---
layout: post
title:  "React Focus Trap"
tags: [ reactjs,navigation,tab,loop,queue,typescript,javascript,web ]
featured_image_thumbnail: 
featured_image:
featured: false
---

Working on a React app I had to implement a behaviour where tab navigation would loop between focusable elements inside a container. By default, web browsers switch the focus between all the interactable elements on page (such as `<a>`, `<input>` or containers with `tabIndex` attribute) based on their position in DOM. Limitting the navigation "context" to a subset of DOM is called "focus trap".

The task seemed trivial and I was hoping to find some generic single-function/component solutions on the web but, alas, only stumbled upon a couple of beefy npm packages and esoteric discussions.

So I'm sharing the solution I've come up with here; hopefully it will be useful as a starting point or inspiration for someone.

```tsx
import { useRef, useEffect } from "react";

type Props = {
    children: Array<JSX.Element | null>;
};

export const FocusTrap = (props: Props) => {
    const ref = useRef<HTMLSpanElement>(null);

    useEffect(() => {
        let firstElement: (HTMLElement | undefined);
        let lastElement: (HTMLElement | undefined);
        if (ref.current != null)
            for (const element of ref.current.querySelectorAll<HTMLElement>("[tabindex]"))
                considerElement(element);
        firstElement?.addEventListener("keydown", handleKeyOnFirst);
        lastElement?.addEventListener("keydown", handleKeyOnLast);
        return () => {
            firstElement?.removeEventListener("keydown", handleKeyOnFirst);
            lastElement?.removeEventListener("keydown", handleKeyOnLast);
        };

        function considerElement(element: HTMLElement) {
            // @ts-ignore
            if (!element.checkVisibility()) return;
            if (firstElement === undefined) firstElement = element;
            else lastElement = element;
        }

        function handleKeyOnFirst(event: KeyboardEvent) {
            if (event.key !== "Tab" || !event.shiftKey) return;
            event.preventDefault();
            lastElement?.focus();
        }

        function handleKeyOnLast(event: KeyboardEvent) {
            if (event.key !== "Tab" || event.shiftKey) return;
            event.preventDefault();
            firstElement?.focus();
        }
    }, [props.children]);

    return <span ref={ref}>{props.children}</span>;
};
```

The trap component scans underlying content finding first and last visible elements with `tabIndex` attribute and overrides Tab/Shift+Tab behavior for them. It can be used as follows:

```tsx
<FocusTrap>
    <ModalOrOtherContentToTrapFocus/>
</FocusTrap>
```

Be aware, that at the time of writing `element.checkVisibility()` function is only [available in Chrome](https://bugs.chromium.org/p/chromium/issues/detail?id=1309533), hence the `@ts-ignore` suppression. Check the [related StackOverflow thread](https://stackoverflow.com/questions/19669786) for other options to check visibility of an element.
