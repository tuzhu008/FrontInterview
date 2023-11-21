# updateProperties

```js
function updateProperties(
  domElement: Element,
  tag: string,
  lastProps: Object,
  nextProps: Object,
): void {
  switch (tag) {
    case 'div':
    case 'span':
    case 'svg':
    case 'path':
    case 'a':
    case 'g':
    case 'p':
    case 'li': {
      // Fast track the most common tag types
      break;
    }
    case 'input': {
      let name = null;
      let type = null;
      let value = null;
      let defaultValue = null;
      let lastDefaultValue = null;
      let checked = null;
      let defaultChecked = null;
      for (const propKey in lastProps) {
        const lastProp = lastProps[propKey];
        if (lastProps.hasOwnProperty(propKey) && lastProp != null) {
          switch (propKey) {
            case 'checked': {
              break;
            }
            case 'value': {
              // This is handled by updateWrapper below.
              break;
            }
            case 'defaultValue': {
              lastDefaultValue = lastProp;
            }
            // defaultChecked and defaultValue are ignored by setProp
            // Fallthrough
            default: {
              if (!nextProps.hasOwnProperty(propKey))
                setProp(domElement, tag, propKey, null, nextProps, lastProp);
            }
          }
        }
      }
      for (const propKey in nextProps) {
        const nextProp = nextProps[propKey];
        const lastProp = lastProps[propKey];
        if (
          nextProps.hasOwnProperty(propKey) &&
          (nextProp != null || lastProp != null)
        ) {
          switch (propKey) {
            case 'type': {
              type = nextProp;
              break;
            }
            case 'name': {
              name = nextProp;
              break;
            }
            case 'checked': {
              checked = nextProp;
              break;
            }
            case 'defaultChecked': {
              defaultChecked = nextProp;
              break;
            }
            case 'value': {
              value = nextProp;
              break;
            }
            case 'defaultValue': {
              defaultValue = nextProp;
              break;
            }
            case 'children':
            case 'dangerouslySetInnerHTML': {
              if (nextProp != null) {
                throw new Error(
                  `${tag} is a void element tag and must neither have \`children\` nor ` +
                    'use `dangerouslySetInnerHTML`.',
                );
              }
              break;
            }
            default: {
              if (nextProp !== lastProp)
                setProp(
                  domElement,
                  tag,
                  propKey,
                  nextProp,
                  nextProps,
                  lastProp,
                );
            }
          }
        }
      }

      if (__DEV__) {
        const wasControlled =
          lastProps.type === 'checkbox' || lastProps.type === 'radio'
            ? lastProps.checked != null
            : lastProps.value != null;
        const isControlled =
          nextProps.type === 'checkbox' || nextProps.type === 'radio'
            ? nextProps.checked != null
            : nextProps.value != null;

        if (
          !wasControlled &&
          isControlled &&
          !didWarnUncontrolledToControlled
        ) {
          console.error(
            'A component is changing an uncontrolled input to be controlled. ' +
              'This is likely caused by the value changing from undefined to ' +
              'a defined value, which should not happen. ' +
              'Decide between using a controlled or uncontrolled input ' +
              'element for the lifetime of the component. More info: https://reactjs.org/link/controlled-components',
          );
          didWarnUncontrolledToControlled = true;
        }
        if (
          wasControlled &&
          !isControlled &&
          !didWarnControlledToUncontrolled
        ) {
          console.error(
            'A component is changing a controlled input to be uncontrolled. ' +
              'This is likely caused by the value changing from a defined to ' +
              'undefined, which should not happen. ' +
              'Decide between using a controlled or uncontrolled input ' +
              'element for the lifetime of the component. More info: https://reactjs.org/link/controlled-components',
          );
          didWarnControlledToUncontrolled = true;
        }
      }

      // Update the wrapper around inputs *after* updating props. This has to
      // happen after updating the rest of props. Otherwise HTML5 input validations
      // raise warnings and prevent the new value from being assigned.
      updateInput(
        domElement,
        value,
        defaultValue,
        lastDefaultValue,
        checked,
        defaultChecked,
        type,
        name,
      );
      return;
    }
    case 'select': {
      let value = null;
      let defaultValue = null;
      let multiple = null;
      let wasMultiple = null;
      for (const propKey in lastProps) {
        const lastProp = lastProps[propKey];
        if (lastProps.hasOwnProperty(propKey) && lastProp != null) {
          switch (propKey) {
            case 'value': {
              // This is handled by updateWrapper below.
              break;
            }
            // defaultValue are ignored by setProp
            case 'multiple': {
              wasMultiple = lastProp;
              // TODO: Move special case in here from setProp.
            }
            // Fallthrough
            default: {
              if (!nextProps.hasOwnProperty(propKey))
                setProp(domElement, tag, propKey, null, nextProps, lastProp);
            }
          }
        }
      }
      for (const propKey in nextProps) {
        const nextProp = nextProps[propKey];
        const lastProp = lastProps[propKey];
        if (
          nextProps.hasOwnProperty(propKey) &&
          (nextProp != null || lastProp != null)
        ) {
          switch (propKey) {
            case 'value': {
              value = nextProp;
              // This is handled by updateSelect below.
              break;
            }
            case 'defaultValue': {
              defaultValue = nextProp;
              break;
            }
            case 'multiple': {
              multiple = nextProp;
              // TODO: Just move the special case in here from setProp.
            }
            // Fallthrough
            default: {
              if (nextProp !== lastProp)
                setProp(
                  domElement,
                  tag,
                  propKey,
                  nextProp,
                  nextProps,
                  lastProp,
                );
            }
          }
        }
      }
      // <select> value update needs to occur after <option> children
      // reconciliation
      updateSelect(domElement, value, defaultValue, multiple, wasMultiple);
      return;
    }
    case 'textarea': {
      let value = null;
      let defaultValue = null;
      for (const propKey in lastProps) {
        const lastProp = lastProps[propKey];
        if (
          lastProps.hasOwnProperty(propKey) &&
          lastProp != null &&
          !nextProps.hasOwnProperty(propKey)
        ) {
          switch (propKey) {
            case 'value': {
              // This is handled by updateTextarea below.
              break;
            }
            case 'children': {
              // TODO: This doesn't actually do anything if it updates.
              break;
            }
            // defaultValue is ignored by setProp
            default: {
              setProp(domElement, tag, propKey, null, nextProps, lastProp);
            }
          }
        }
      }
      for (const propKey in nextProps) {
        const nextProp = nextProps[propKey];
        const lastProp = lastProps[propKey];
        if (
          nextProps.hasOwnProperty(propKey) &&
          (nextProp != null || lastProp != null)
        ) {
          switch (propKey) {
            case 'value': {
              value = nextProp;
              // This is handled by updateTextarea below.
              break;
            }
            case 'defaultValue': {
              defaultValue = nextProp;
              break;
            }
            case 'children': {
              // TODO: This doesn't actually do anything if it updates.
              break;
            }
            case 'dangerouslySetInnerHTML': {
              if (nextProp != null) {
                // TODO: Do we really need a special error message for this. It's also pretty blunt.
                throw new Error(
                  '`dangerouslySetInnerHTML` does not make sense on <textarea>.',
                );
              }
              break;
            }
            default: {
              if (nextProp !== lastProp)
                setProp(
                  domElement,
                  tag,
                  propKey,
                  nextProp,
                  nextProps,
                  lastProp,
                );
            }
          }
        }
      }
      updateTextarea(domElement, value, defaultValue);
      return;
    }
    case 'option': {
      for (const propKey in lastProps) {
        const lastProp = lastProps[propKey];
        if (
          lastProps.hasOwnProperty(propKey) &&
          lastProp != null &&
          !nextProps.hasOwnProperty(propKey)
        ) {
          switch (propKey) {
            case 'selected': {
              // TODO: Remove support for selected on option.
              (domElement: any).selected = false;
              break;
            }
            default: {
              setProp(domElement, tag, propKey, null, nextProps, lastProp);
            }
          }
        }
      }
      for (const propKey in nextProps) {
        const nextProp = nextProps[propKey];
        const lastProp = lastProps[propKey];
        if (
          nextProps.hasOwnProperty(propKey) &&
          nextProp !== lastProp &&
          (nextProp != null || lastProp != null)
        ) {
          switch (propKey) {
            case 'selected': {
              // TODO: Remove support for selected on option.
              (domElement: any).selected =
                nextProp &&
                typeof nextProp !== 'function' &&
                typeof nextProp !== 'symbol';
              break;
            }
            default: {
              setProp(domElement, tag, propKey, nextProp, nextProps, lastProp);
            }
          }
        }
      }
      return;
    }
    case 'img':
    case 'link':
    case 'area':
    case 'base':
    case 'br':
    case 'col':
    case 'embed':
    case 'hr':
    case 'keygen':
    case 'meta':
    case 'param':
    case 'source':
    case 'track':
    case 'wbr':
    case 'menuitem': {
      // Void elements
      for (const propKey in lastProps) {
        const lastProp = lastProps[propKey];
        if (
          lastProps.hasOwnProperty(propKey) &&
          lastProp != null &&
          !nextProps.hasOwnProperty(propKey)
        ) {
          setProp(domElement, tag, propKey, null, nextProps, lastProp);
        }
      }
      for (const propKey in nextProps) {
        const nextProp = nextProps[propKey];
        const lastProp = lastProps[propKey];
        if (
          nextProps.hasOwnProperty(propKey) &&
          nextProp !== lastProp &&
          (nextProp != null || lastProp != null)
        ) {
          switch (propKey) {
            case 'children':
            case 'dangerouslySetInnerHTML': {
              if (nextProp != null) {
                // TODO: Can we make this a DEV warning to avoid this deny list?
                throw new Error(
                  `${tag} is a void element tag and must neither have \`children\` nor ` +
                    'use `dangerouslySetInnerHTML`.',
                );
              }
              break;
            }
            // defaultChecked and defaultValue are ignored by setProp
            default: {
              setProp(domElement, tag, propKey, nextProp, nextProps, lastProp);
            }
          }
        }
      }
      return;
    }
    default: {
      if (isCustomElement(tag, nextProps)) {
        for (const propKey in lastProps) {
          const lastProp = lastProps[propKey];
          if (
            lastProps.hasOwnProperty(propKey) &&
            lastProp != null &&
            !nextProps.hasOwnProperty(propKey)
          ) {
            setPropOnCustomElement(
              domElement,
              tag,
              propKey,
              null,
              nextProps,
              lastProp,
            );
          }
        }
        for (const propKey in nextProps) {
          const nextProp = nextProps[propKey];
          const lastProp = lastProps[propKey];
          if (
            nextProps.hasOwnProperty(propKey) &&
            nextProp !== lastProp &&
            (nextProp != null || lastProp != null)
          ) {
            setPropOnCustomElement(
              domElement,
              tag,
              propKey,
              nextProp,
              nextProps,
              lastProp,
            );
          }
        }
        return;
      }
    }
  }

  for (const propKey in lastProps) {
    const lastProp = lastProps[propKey];
    if (
      lastProps.hasOwnProperty(propKey) &&
      lastProp != null &&
      !nextProps.hasOwnProperty(propKey)
    ) {
      setProp(domElement, tag, propKey, null, nextProps, lastProp);
    }
  }
  for (const propKey in nextProps) {
    const nextProp = nextProps[propKey];
    const lastProp = lastProps[propKey];
    if (
      nextProps.hasOwnProperty(propKey) &&
      nextProp !== lastProp &&
      (nextProp != null || lastProp != null)
    ) {
      setProp(domElement, tag, propKey, nextProp, nextProps, lastProp);
    }
  }
}
```