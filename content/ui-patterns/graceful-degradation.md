---
title: Graceful degradation (new name TBD)
description: How to preserve functionality and quality user experience when critical services are unavailable.
---

<!-- TODO: Reconsider the title of this document. "Graceful degradation" usually refers to legacy browser support -->

<!-- TODO: Write a separate document with guidelines about how to craft helpful error message content and UI -->

<Note>These guidelines focus on cases where there is a service outage. They are not intended as general design guidance for error handling.</Note>

When there is a failure that prevents the user from seeing or interacting with something, we aim to provide as good of a user experience as possible. Instead of blocking an entire page from rendering, we should give users as much functionality as possible.

We should communicate that there is an problem and guide users around that problem as much as possible. We should not try to conceal or downplay that something is wrong. Ideally we can strike a balance and give the user just the right amount of context without overwhelming them with error messages. A page with too many error messages could communicate an unnecessarily reactionary and negative tone.

## Global system notification

<!-- TODO: iterate on global system notification design and guidelines -->

<img width="456" alt="Image of a page with a banner at the top explaining an error" src="https://github.com/primer/react/assets/2313998/80b52dc9-0214-440d-8419-afec7ea21da4" />

<!-- TODO: determine if the "warning" variant is the right path forward -->

If there is an outage or some other critical error that will break a normal workflow, show a [flash](/components/flash) at the top of the page above the global navigation. Having a global banner helps set the expectation that some parts of the usual UI might be missing or broken. Default to using the "warning" variant of the flash.

Explain what's wrong and, if possible, link to a page with more detailed information. For example, if there's a database outage, we could link to the [GitHub status page](https://www.githubstatus.com/).

In addition to a global banner, we should inform users about availability issues in context. The following guidelines discuss how to handle outages in the context of the affected UI.

## Degraded page content

If part of the UI cannot be rendered or would be rendered without critical information, default to not rendering it at all. However, it may be replaced with an error message if it would be disorienting to suppress rendering altogether. Before making a decision about how to handle UI with unavailable content, check if it already has guidelines on handling errors or empty states.

### Removing UI

You can remove affected UI if it's not critical to core workflows and it wouldn't be confusing the render the rest of the page without that UI. For example: it's ok to hide the reactions button.

Examples of UI that might result in a disorienting experience if removed:

- The comment box on issues and pull requests
- The "New issue" button on the issues page
- Submit buttons on forms

### Replacing with a blankslate component

<img width="960" alt="Two images: 1. comment box; 2. blankslate in place of comment box" src="https://github.com/primer/react/assets/2313998/a9670e6c-b3ae-42db-9c32-99e807bd7b85">

If the affected area is large enough, replace the affected UI with a [blankstate](/components/blankslate) component that explains why the expected UI isn't there.

### Replacing with a message

<img width="960" alt="Two images: 1. Image of Memex table with populated content cells; 2. Image of Memex table with missing content message in cells" src="https://github.com/primer/react/assets/2313998/2ddfad87-b445-4524-8a7a-64f4fca2ee09">

Smaller parts of the UI that cannot be accurately rendered but are too important to exclude entirely can often be replaced with a short error message.

<DoDontContainer>
  <Do>
    <img
      src="https://github.com/primer/react/assets/2313998/4d6e4bd2-bc77-4a8d-be73-1f363b06bbd1"
      alt="Image of Memex table with missing content message in cells"
      width="456"
    />
    <Caption>Render an error message in place of the content.</Caption>
  </Do>
  <Dont>
    <img
      width="456"
      alt="Image of Memex table with 'undefined of undefined'"
      src="https://github.com/primer/react/assets/2313998/47718419-0dee-4c64-81aa-5cd1ca232fe8"
    />
    <Caption>Don't attempt to render UI that is missing critical information.</Caption>
  </Dont>
</DoDontContainer>

<!-- TODO: I'm not sure if `fg.warning` should be used by default. It could be very loud. -->
<!-- By default, show a warning icon before the message and color the icon and the message with `fg.warning`. If the page is likely to have many pieces of content. -->

<!-- TODO: come up with a realistic example of a lot of error messages being rendered in a small area -->
<!-- Rendering too many error messages in a small area will be jarring and over-emphasize that something is wrong. If there's an area of the page that is mostly unavailable, either supress rendering entirely, or replace the entire area with a [blankslate](/components/blankslate) component if it is too critical to not be rendered. -->

<DoDontContainer>
  <Do>
    <img
      width="456"
      alt="Image TBD"
      src="https://github.com/primer/react/assets/2313998/07bf9e3d-e36a-45fb-ad3e-e8960192b443"
    />
    <Caption>Replace an area with a lot of broken parts with a blankslate.</Caption>
  </Do>
  <Dont>
    <img
      width="456"
      alt="Image TBD"
      src="https://github.com/primer/react/assets/2313998/670e0edb-bdca-4bc8-a67c-cdb2f7f0c399"
    />
    <Caption>Don't render a bunch of error messages in a small area.</Caption>
  </Dont>
</DoDontContainer>

### Handling unavailable count numbers

![Three images of repo tabs: 1. Tabs have counts; 2. Tabs don't have counts; 3. Tabs don't have counts, and but they show a tooltip.]()

When the data required to calculate a count is unavailable, default to hiding the number. If the count is shown inside of an interactive element, a tooltip may be displayed on focus and hover to explain the missing count.

<!-- TODO: Decide if we need to come up with a pattern for replacing (instead of hiding) numbers in an unavailable count. -->

## Degraded navigation

### Global navigation

<!-- TODO: Figure out examples of global nav content that may be affected by outages -->

![Example of a degraded global nav]()

Never suppress rendering of the global navigation, but items within the navigation affected by a system error may. Rendering a page without global navigation could make a user feel stuck. A link to the home page should always be available, even if the contents of the homepage will be broken.

### Other page navigation

<!-- TODO: Figure out examples of page nav content that may be affected by outages -->

![Example of a degraded repo nav]()

There may be cases where a page has to degrade it's own navigation. Common types of page navigation include [tabs](/components/tab-nav) (for example, a repo page) and [sidebar navigation](/components/nav-list) (for example, settings pages).

## Degraded actions

### Handling non-functional[buttons](/components/button)

![Decision tree for how to handle a non-functional button]()

#### Removing a non-functional button

Default to removing non-functional buttons from the UI. Don't remove buttons that are critical to a user's workflow—it may be disorienting.

<DoDontContainer>
  <Do>
    <img
      width="456"
      alt="New issue button hidden"
      src="https://github.com/primer/react/assets/2313998/e1c8f524-885f-42a7-86f4-707fc0d72eec"
    />
    <Caption>Hide non-critical buttons.</Caption>
  </Do>
  <Dont>
    <img
      width="456"
      alt="Comment react button hidden"
      src="https://github.com/primer/react/assets/2313998/b7d94241-3c03-4cfd-91e2-1d9d11a2827b"
    />
    <Caption>Don't hide buttons that part of core workflows.</Caption>
  </Dont>
</DoDontContainer>

#### Indicating a button is non-functional without disabling it

<!-- TODO: Decide whether this should be added to the component API, or use the existing Button API. If so, maybe its it's own component that composes Button+Tooltip+Dialog. -->

<img width="456" alt="A default button and an inert button" src="https://github.com/primer/react/assets/2313998/ca435c11-f8dc-4a63-a765-5bac04ed9997">

If a button is too critical to be omitted and responds to user input by showing more info about why it's non-functional, use an inert button. An inert button has the same styles as a disabled button, but it still reponds to user input.

An inert button is the accessible option because buttons may not be disabled if they respond to user input. For example, if a button shows a tooltip when hovered or focused, or a dialog when clicked.

There are two recommended ways an inert button should respond to user input:

1. **Show a dialog on click:** When a user tries to activate an inert the button, open a [dialog](/components/dialog) that explains why the action cannot be completed and give them a path forward if possible. It is required to provide some kind of feedback when a user clicks a button.
2. **Show a tooltip on hover or focus:** Before a user tries to activate a non-functional control, a [tooltip](/components/tooltip) with additional context may be displayed on hover or focus. It is _not_ required to respond to hover and focus.

<img width="960" alt="A video of a 'broken' button that shows a tooltip and opens an error dialog when activated" src="https://github.com/primer/react/assets/2313998/d1da7fc9-bfd9-431e-a774-c1c1c08bb093">

#### Disabling a non-functional button

<!-- TODO: Come up with a visual example where it's safe to disable the button. -->

![Example of a button that can be disabled because it has sufficient context explaining why it is disabled]()

If a button is non-functional and there is already enough context for the user to understand why it's disabled, there's no need to respond to user input, so the button can safely be disabled.

For more info, see the guidance on [disabling links and buttons](https://accessibility-playbook.github.com/link-and-button-guidance#disabling-links-and-buttons) from GitHub's Accessibility Playbook (only available to GitHub staff).

### Handling [action menus](/components/action-menu) with non-functional items

<img width="960" alt="Three examples: 1. An action menu that works just fine; 2. An action menu with some items disabled; 3. An action menu with all items disabled" src="https://github.com/primer/react/assets/2313998/28f13e91-903c-4784-b3af-86eb8393aaa6">

Show the menu items, but disable them and show a message explaining why they're disabled.

If an action menu contains menu items that are very closely related (for example: the action menu for adding a file to a repo) and we can detect that none of the menu items are available, then indicate a problem on the button that triggers the action menu. Use an icon in the button to indicate that something is wrong, and (optionally) show a tooltip on hover or focus.

<!--
It's unclear if we'll have any use cases for the following components,
so I'm leaving them out until we do.

### Tabs

### Form controls

### Links

### Segmented controls

### Toggle switches
-->

<!--
TODO: uncomment this section when/if we add loading state guidelines

## Waiting for a timeout

There could be cases where we're waiting for data to load before determining that it's unavailable. In this case, refer to Primer's general loading state guidelines or the component-specific loading state guidelines (if the component has loading states).
-->

## Entire page fails to load

If an error is critical enough to prevent the entire page from loading, fall back to showing a default error page. For example, the HTTP 500 server error page.

## CLI

It's confusing and frustrating when a command silently fails. If a command fails, immediately return an error message or wait for a timeout to expire and then return an error message.

## Accessibility

Never disable an interactive control that is non-functional due to availability issues.

A common (but inaccessible) pattern is to show a tooltip with more information when a user hovers an error message or a disabled button. However, tooltips may only be used on focusable elements.

<!-- TODO: determine what other a11y considerations that need to be documented -->

## Related links

- [Blankslate](/components/blankslate)
- [Dialog](/components/dialog)
- [Tooltip](/components/tooltip)
- [Messaging]()
- [Empty states]()
- [Error message content]()
- [Loading states]()

<!--
Potential new patterns we'll need:

- Button/IconButton "inert" state
- ActionList.Item "inert" state
  - Items w/ just a title
  - Items w/ title + description
- Label, CounterLabel, StatusLabel unavailable state
- Simple error message text (warning icon + text)
-->