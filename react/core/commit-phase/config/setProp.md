# setProp

```js
function setProp(
  domElement: Element,
  tag: string,
  key: string,
  value: mixed,
  props: any,
  prevValue: mixed,
): void {
  switch (key) {
    case 'children': {
      if (typeof value === 'string') {
        // Avoid setting initial textContent when the text is empty. In IE11 setting
        // textContent on a <textarea> will cause the placeholder to not
        // show within the <textarea> until it has been focused and blurred again.
        // https://github.com/facebook/react/issues/6731#issuecomment-254874553
        const canSetTextContent =
          (!enableHostSingletons || tag !== 'body') &&
          (tag !== 'textarea' || value !== '');
        if (canSetTextContent) {
          setTextContent(domElement, value);
        }
      } else if (typeof value === 'number') {
        const canSetTextContent = !enableHostSingletons || tag !== 'body';
        if (canSetTextContent) {
          setTextContent(domElement, '' + value);
        }
      }
      break;
    }
    // These are very common props and therefore are in the beginning of the switch.
    // TODO: aria-label is a very common prop but allows booleans so is not like the others
    // but should ideally go in this list too.
    case 'className':
      setValueForKnownAttribute(domElement, 'class', value);
      break;
    case 'tabIndex':
      // This has to be case sensitive in SVG.
      setValueForKnownAttribute(domElement, 'tabindex', value);
      break;
    case 'dir':
    case 'role':
    case 'viewBox':
    case 'width':
    case 'height': {
      setValueForKnownAttribute(domElement, key, value);
      break;
    }
    case 'style': {
      setValueForStyles(domElement, value, prevValue);
      break;
    }
    // These attributes accept URLs. These must not allow javascript: URLS.
    case 'src':
    case 'href': {
      if (enableFilterEmptyStringAttributesDOM) {
        if (value === '') {
          domElement.removeAttribute(key);
          break;
        }
      }
      if (
        value == null ||
        typeof value === 'function' ||
        typeof value === 'symbol' ||
        typeof value === 'boolean'
      ) {
        domElement.removeAttribute(key);
        break;
      }
      // `setAttribute` with objects becomes only `[object]` in IE8/9,
      // ('' + value) makes it output the correct toString()-value.
      const sanitizedValue = (sanitizeURL(
        enableTrustedTypesIntegration ? value : '' + (value: any),
      ): any);
      domElement.setAttribute(key, sanitizedValue);
      break;
    }
    case 'action':
    case 'formAction': {
      // TODO: Consider moving these special cases to the form, input and button tags.
      if (enableFormActions) {
        if (typeof value === 'function') {
          // Set a javascript URL that doesn't do anything. We don't expect this to be invoked
          // because we'll preventDefault, but it can happen if a form is manually submitted or
          // if someone calls stopPropagation before React gets the event.
          // If CSP is used to block javascript: URLs that's fine too. It just won't show this
          // error message but the URL will be logged.
          domElement.setAttribute(
            key,
            // eslint-disable-next-line no-script-url
            "javascript:throw new Error('" +
              'A React form was unexpectedly submitted. If you called form.submit() manually, ' +
              "consider using form.requestSubmit() instead. If you're trying to use " +
              'event.stopPropagation() in a submit event handler, consider also calling ' +
              'event.preventDefault().' +
              "')",
          );
          break;
        } else if (typeof prevValue === 'function') {
          // When we're switching off a Server Action that was originally hydrated.
          // The server control these fields during SSR that are now trailing.
          // The regular diffing doesn't apply since we compare against the previous props.
          // Instead, we need to force them to be set to whatever they should be now.
          // This would be a lot cleaner if we did this whole fork in the per-tag approach.
          if (key === 'formAction') {
            if (tag !== 'input') {
              // Setting the name here isn't completely safe for inputs if this is switching
              // to become a radio button. In that case we let the tag based override take
              // control.
              setProp(domElement, tag, 'name', props.name, props, null);
            }
            setProp(
              domElement,
              tag,
              'formEncType',
              props.formEncType,
              props,
              null,
            );
            setProp(
              domElement,
              tag,
              'formMethod',
              props.formMethod,
              props,
              null,
            );
            setProp(
              domElement,
              tag,
              'formTarget',
              props.formTarget,
              props,
              null,
            );
          } else {
            setProp(domElement, tag, 'encType', props.encType, props, null);
            setProp(domElement, tag, 'method', props.method, props, null);
            setProp(domElement, tag, 'target', props.target, props, null);
          }
        }
      }
      if (
        value == null ||
        (!enableFormActions && typeof value === 'function') ||
        typeof value === 'symbol' ||
        typeof value === 'boolean'
      ) {
        domElement.removeAttribute(key);
        break;
      }
      // `setAttribute` with objects becomes only `[object]` in IE8/9,
      // ('' + value) makes it output the correct toString()-value.
      const sanitizedValue = (sanitizeURL(
        enableTrustedTypesIntegration ? value : '' + (value: any),
      ): any);
      domElement.setAttribute(key, sanitizedValue);
      break;
    }
    case 'onClick': {
      // TODO: This cast may not be sound for SVG, MathML or custom elements.
      if (value != null) {
        trapClickOnNonInteractiveElement(((domElement: any): HTMLElement));
      }
      break;
    }
    case 'onScroll': {
      if (value != null) {
        listenToNonDelegatedEvent('scroll', domElement);
      }
      break;
    }
    case 'dangerouslySetInnerHTML': {
      if (value != null) {
        if (typeof value !== 'object' || !('__html' in value)) {
          throw new Error(
            '`props.dangerouslySetInnerHTML` must be in the form `{__html: ...}`. ' +
              'Please visit https://reactjs.org/link/dangerously-set-inner-html ' +
              'for more information.',
          );
        }
        const nextHtml: any = value.__html;
        if (nextHtml != null) {
          if (props.children != null) {
            throw new Error(
              'Can only set one of `children` or `props.dangerouslySetInnerHTML`.',
            );
          }
          if (disableIEWorkarounds) {
            domElement.innerHTML = nextHtml;
          } else {
            setInnerHTML(domElement, nextHtml);
          }
        }
      }
      break;
    }
    // Note: `option.selected` is not updated if `select.multiple` is
    // disabled with `removeAttribute`. We have special logic for handling this.
    case 'multiple': {
      (domElement: any).multiple =
        value && typeof value !== 'function' && typeof value !== 'symbol';
      break;
    }
    case 'muted': {
      (domElement: any).muted =
        value && typeof value !== 'function' && typeof value !== 'symbol';
      break;
    }
    case 'suppressContentEditableWarning':
    case 'suppressHydrationWarning':
    case 'defaultValue': // Reserved
    case 'defaultChecked':
    case 'innerHTML': {
      // Noop
      break;
    }
    case 'autoFocus': {
      // We polyfill it separately on the client during commit.
      // We could have excluded it in the property list instead of
      // adding a special case here, but then it wouldn't be emitted
      // on server rendering (but we *do* want to emit it in SSR).
      break;
    }
    case 'xlinkHref': {
      if (
        value == null ||
        typeof value === 'function' ||
        typeof value === 'boolean' ||
        typeof value === 'symbol'
      ) {
        domElement.removeAttribute('xlink:href');
        break;
      }
      // `setAttribute` with objects becomes only `[object]` in IE8/9,
      // ('' + value) makes it output the correct toString()-value.
      const sanitizedValue = (sanitizeURL(
        enableTrustedTypesIntegration ? value : '' + (value: any),
      ): any);
      domElement.setAttributeNS(xlinkNamespace, 'xlink:href', sanitizedValue);
      break;
    }
    case 'contentEditable':
    case 'spellCheck':
    case 'draggable':
    case 'value':
    case 'autoReverse':
    case 'externalResourcesRequired':
    case 'focusable':
    case 'preserveAlpha': {
      // Booleanish String
      // These are "enumerated" attributes that accept "true" and "false".
      // In React, we let users pass `true` and `false` even though technically
      // these aren't boolean attributes (they are coerced to strings).
      // The SVG attributes are case-sensitive. Since the HTML attributes are
      // insensitive they also work even though we canonically use lower case.
      if (
        value != null &&
        typeof value !== 'function' &&
        typeof value !== 'symbol'
      ) {
        domElement.setAttribute(
          key,
          enableTrustedTypesIntegration ? (value: any) : '' + (value: any),
        );
      } else {
        domElement.removeAttribute(key);
      }
      break;
    }
    // Boolean
    case 'allowFullScreen':
    case 'async':
    case 'autoPlay':
    case 'controls':
    case 'default':
    case 'defer':
    case 'disabled':
    case 'disablePictureInPicture':
    case 'disableRemotePlayback':
    case 'formNoValidate':
    case 'hidden':
    case 'loop':
    case 'noModule':
    case 'noValidate':
    case 'open':
    case 'playsInline':
    case 'readOnly':
    case 'required':
    case 'reversed':
    case 'scoped':
    case 'seamless':
    case 'itemScope': {
      if (value && typeof value !== 'function' && typeof value !== 'symbol') {
        domElement.setAttribute(key, '');
      } else {
        domElement.removeAttribute(key);
      }
      break;
    }
    // Overloaded Boolean
    case 'capture':
    case 'download': {
      // An attribute that can be used as a flag as well as with a value.
      // When true, it should be present (set either to an empty string or its name).
      // When false, it should be omitted.
      // For any other value, should be present with that value.
      if (value === true) {
        domElement.setAttribute(key, '');
      } else if (
        value !== false &&
        value != null &&
        typeof value !== 'function' &&
        typeof value !== 'symbol'
      ) {
        domElement.setAttribute(key, (value: any));
      } else {
        domElement.removeAttribute(key);
      }
      break;
    }
    case 'cols':
    case 'rows':
    case 'size':
    case 'span': {
      // These are HTML attributes that must be positive numbers.
      if (
        value != null &&
        typeof value !== 'function' &&
        typeof value !== 'symbol' &&
        !isNaN(value) &&
        (value: any) >= 1
      ) {
        domElement.setAttribute(key, (value: any));
      } else {
        domElement.removeAttribute(key);
      }
      break;
    }
    case 'rowSpan':
    case 'start': {
      // These are HTML attributes that must be numbers.
      if (
        value != null &&
        typeof value !== 'function' &&
        typeof value !== 'symbol' &&
        !isNaN(value)
      ) {
        domElement.setAttribute(key, (value: any));
      } else {
        domElement.removeAttribute(key);
      }
      break;
    }
    case 'xlinkActuate':
      setValueForNamespacedAttribute(
        domElement,
        xlinkNamespace,
        'xlink:actuate',
        value,
      );
      break;
    case 'xlinkArcrole':
      setValueForNamespacedAttribute(
        domElement,
        xlinkNamespace,
        'xlink:arcrole',
        value,
      );
      break;
    case 'xlinkRole':
      setValueForNamespacedAttribute(
        domElement,
        xlinkNamespace,
        'xlink:role',
        value,
      );
      break;
    case 'xlinkShow':
      setValueForNamespacedAttribute(
        domElement,
        xlinkNamespace,
        'xlink:show',
        value,
      );
      break;
    case 'xlinkTitle':
      setValueForNamespacedAttribute(
        domElement,
        xlinkNamespace,
        'xlink:title',
        value,
      );
      break;
    case 'xlinkType':
      setValueForNamespacedAttribute(
        domElement,
        xlinkNamespace,
        'xlink:type',
        value,
      );
      break;
    case 'xmlBase':
      setValueForNamespacedAttribute(
        domElement,
        xmlNamespace,
        'xml:base',
        value,
      );
      break;
    case 'xmlLang':
      setValueForNamespacedAttribute(
        domElement,
        xmlNamespace,
        'xml:lang',
        value,
      );
      break;
    case 'xmlSpace':
      setValueForNamespacedAttribute(
        domElement,
        xmlNamespace,
        'xml:space',
        value,
      );
      break;
    // Properties that should not be allowed on custom elements.
    case 'is': {
      // TODO: We shouldn't actually set this attribute, because we've already
      // passed it to createElement. We don't also need the attribute.
      // However, our tests currently query for it so it's plausible someone
      // else does too so it's break.
      setValueForAttribute(domElement, 'is', value);
      break;
    }
    case 'innerText':
    case 'textContent':
      if (enableCustomElementPropertySupport) {
        break;
      }
    // Fall through
    default: {
      if (
        key.length > 2 &&
        (key[0] === 'o' || key[0] === 'O') &&
        (key[1] === 'n' || key[1] === 'N')
      ) {
        //..
      } else {
        const attributeName = getAttributeAlias(key);
        setValueForAttribute(domElement, attributeName, value);
      }
    }
  }
}
```