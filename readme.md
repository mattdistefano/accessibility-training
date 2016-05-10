# Accessibility training

## What is accessibility? 

Accessibility can refer to two distinct, but related concepts. First, it can be an expression of how well people with disabilities are able to utilize (or, _access_) a product or service. Second, it can refer to the practice of designing and developing products and services to be accessible. 

## How do people with disabilities interact with web sites and apps?

Many users with disabilities will take advantage of some sort of assistive technology (AT). AT augments, adjusts, or even entirely replaces the screen/mouse paradigm of interaction. For example, a blind user may utilize a screen reader, which provides a spoken representation of the screen; a user with impaired motor skills might navigate via keyboard rather than mouse; a user with poor eyesight may zoom their browser. 

Note that not all disabilities require the use of AT. A user who is deaf or color-blind or cognitively-impaired may just browse with monitor and mouse, but still encounter content that is difficult or impossible to use. 

Note too that AT is not limited to just desktops or laptops. Touch-based screen readers are available for tablets and smartphones.

## How does accessibility (or lack thereof) impact a disabled user's experience?

In the worst case, poor accessibility can render a site or app totally unusable. Critical content may be unreachable, and critical operations unavailable. 

## What are the relevant standards/guidelines?

WCAG 2.0 is the most widely-recognized guideline for digital accessibility. While it is not currently required by law, courts have identified it as a benchmark for accessibility in lawsuits brought under ADA, and it is likely to be the basis for future legislation. 

WCAG 2.0 is broken into 3 levels - A, AA, and AAA (AAA being the most stringent). We currently try to comply with AA.

## How do we comply?

WCAG compliance is a tricky subject. Because WCAG is intended to describe the requirements for accessible digital applications *in general*, the details of meeting those requirements in a specific technology or design often require additional research and experimentation. Likewise, because WCAG often specifies outcomes instead of implementation details, real-world testing is frequently required. This in turn can be challenging, because UAs and ATs can themselves be buggy, different combinations of UA and AT may produce different results, AT can often be expensive and difficult to learn, etc. 

That said, there's a number of things we can do to promote accessibility in our projects:

1. Address accessibility early by including it in the visual and technical design process
2. Follow best practices and established patterns 
3. Use automation where viable to test for compliance
4. Perform manual testing during development and QA
5. Invite disabled users to participate in testing

## What are the considerations for design?

### Colors and contrast

WCAG requires a minimum contrast ratio of 4.5:1 for normal-sized text, and 3:1 for large text (defined as 14pt - roughly 19px - bold weight, or 18 point - roughly 24px - normal weight). See http://webaim.org/resources/contrastchecker/ for a helpful contrast checker. Choosing high-contrast colors helps keep text readable for users with various vision-related disabilities. (1.4.3)

Avoid the use of color *alone* as a means of conveying information. For example, the error state for a form field may *include* a change in border or label color, but if that is the sole visual indicator, a color-blind user would be unable to perceive the error. Links should either have a non-color-based visual cue (underlining or bolding, for example) in their normal state, or use a color that provides 3:1 contrast against the surrounding text, and have a non-color-based visual cue in their hover/focus state. (1.4.1)

### IA, navigation, and content

Information architecture and navigation should use consistent, predictable patterns as much as possible. For example, when navigation elements are present on multiple pages, they should appear in the same position within each page, and the relative order of their links should not vary between pages. Likewise, other pieces of content or functionality that are used on multiple pages should be identified consistently. For example, a component providing global search functionality should use the same label on every page; it should not be labeled 'Search' on one page and 'Find' on another. This predictability is beneficial to all users, but particularly to users of AT, who otherwise would have to spend time learning the structure of each new page before they could use it effectively. (3.2.3, 3.2.4)

Pages should typically also be discoverable through at least two ways (navigation, links within pages, search, site map, etc). This gives users with disabilities the option to use whichever mechanism works best for them, and ensures a fallback is available in case they encounter issues. Note that this is not required for pages like a checkout or confirmation screen that represent a step within a process. (2.4.5)

When linking, be sure the purpose of the link is clear either from its text alone, or the surrounding text. Try to avoid link text like "click here", having multiple links to the same destination with different text, or multiple links to different destinations with the same text, as these can all be confusing. (2.4.4)

All pages should have a descriptive title and descriptive headings should be used to organize content. (2.4.2, 2.4.6)

### Multi-modality

Avoid creating content that relies solely on sensory characteristics that all users might not be able to perceive. For example, instructions to "click the big red button" are not useful to a blind user who can't see the size or color of the buttons on the screen. (1.3.3)

Likewise, when designing interactive functionality, be sure to consider users who cannot operate a mouse. Always include focus states for any clickable element, avoid interfaces that depend entirely on mouse hover, and identify an alternate means of operation for functionality like drag-and-drop (designers and developers may need to collaborate on these sort of interactions). (2.1.1, 2.4.7)

### Text alternatives

All meaningful graphical content should have a text alternative that developers can incorporate into the final product. Note that this includes not just bitmap images and photos, but also icons, charts, graphs, some animations, and potentially other graphical content. (1.1.1)

### Audio and video

TODO

### Animations, moving content, auto-updating content

Any animations that start automatically, are presented alongside other content, and run for more than 5 seconds must include a mechanism allowing them to be paused, stopped, or hidden by the user. An exception can be made when the animation itself is essential to the page. For example, an auto-playing slideshow generally would not be essential, and therefore must be pausable, but a loading spinner does not need to be pausable, as its continued playback is essential to understanding that the page or component is still in a loading state. (2.2.2)

Likewise, if content automatically updates, the user should be given an option to stop, pause, hide, or control the frequency of the updates, unless the updates are essential. (2.2.2)

Finally, avoid [types of flashes known to cause seizures](https://www.w3.org/TR/UNDERSTANDING-WCAG20/seizure-does-not-violate.html) (2.3.1).

### Forms

With few exceptions, all form fields should have visible labels. Placeholder text may also be used, but should not be used in lieu of a label. (1.1.1, 3.3.2)

When forms will be validated prior to submission, visual indication of any validation failures should be provided, along with descriptive error messages. (3.3.1, 3.3.3)

Similar forms should use consistent structure and labeling patterns. For example, if two forms on a site ask for the user's Social Security Number, they should use the same label text and input structure. (3.2.4)

TODO identifying required fields

### Context changes

Avoid introducing context changes (navigation to a new page, tab, or window, or a shift in focus within the page) in unexpected places. For example, navigating to a new page in response to a button or link click is expected, but doing so when a link or button is merely focused is not expected and would produce a very confusing experience for many users. Interacting with a form control *may* produce a context change, but only if the user has been advised of it in advance. For example, a phone number input consisting of three separate fields may auto-advance to the next field when the current field is completed, but instructions advising of this behavior should be included in the page before the form fields. (3.2.1, 3.2.2)

### Time limits and confirmation

TODO

### External documents (Word, PDF, etc)

TODO

## What are the considerations for development?

### Semantic markup

Developers should understand the meaning of different tags and choose them carefully. Remember: HTML is a language for adding structure and meaning to content - it's not just about putting that content on the screen. A surprisingly large number of accessibility issues can be traced back to bad markup choices. Know your tags!

#### Common issues

- Headings should be marked up with `h1` through `h6` tags; don't just style a div
- `table`s should be used always and only for tabular data; don't use them for layout purposes, and don't create table-like structures from non-`table` elements
- `ol` and `ul` should be used for ordered and unordered lists, respectively
- `a` or `button` should be used for navigation and actions within a page

### Document structure

#### Landmarks

Use specific tags and/or the `role` attribute to identify landmarks within a document. For example, the main content area should be marked up with a `main` tag so that supporting AT can skip directly to it. (2.4.1)

#### Headings

Headings should generally appear in order, where each level of heading represents a subheading within the previous heading's content. (1.3.1)

```html
<h1>Level 1</h2>
    <p>Level 1 content</p>
    <h2>Level 1.1</h2>
        <p>Level 1.1 content</p>
        <h3>Level 1.1.1</h2>
            <p>Level 1.1.1 content</p>
    <h2>Level 1.2</h2>
        <p>Level 1.2 content</p>
        <h3>Level 1.2.1</h3>
            <p>Level 1.2.1 content</p>
        <h3>Level 1.2.2</h3>
            <p>Level 1.2.2 content</p>
            <h4>Level 1.2.2.1</h4>
                <p>Level 1.2.2.1 content</p>
```

This allows AT to navigate an accurate outline of the page's content.

#### Reading order

Since AT typically reads elements in the order in which they appear in the DOM, content in the source HTML should typically have a sensible reading order that parallels how it will be presented on screen. So, for example, while it's possible to put a section heading *beneath* its content in the DOM, but visually position it *above* that same content via CSS - don't. (1.3.2)

### Alternative content

Since not all users can see the visual presentation of a site or app, it's important to ensure that any meaningful information that is conveyed visually is also conveyed in an accessible, text-based manner. (1.1.1)

#### Hidden content

Throughout the examples in this document, the `visually-hidden` class will be used. This does not necessarily represent a _specific_ class, but is intended to represent the general concept of a style that hides content from the visual presentation while keeping it available to AT. In most cases, content that is styled with `display: none` or `visibility: hidden` will not be readable by AT. Often, this is what we want, but sometimes we want to keep content accessible, while still hiding it on the screen. In these cases, a `visually-hidden` style is appropriate. 

#### Images

Many developers are already familiar with the `alt` attribute of the `img` tag; this attribute should be present on every `img`, though its content may be an empty string when the image is purely decorative. When a screen reader encounters an `img`, it will read the content of the `alt` attribute (when the `alt` attribute is not present, the file name may be read instead, which is why an empty string should be used for decorative images).

```html
<!-- this is a decorative image -->
<img alt="" src="decorative-image.jpg" />
```

```html
<!-- this is a meaningful image -->
<img alt="information about or equivalent to the image" src="meaningful-image.jpg" />
```

Note that CSS background images do not have alternate text, nor are they typically accessible to AT. This makes them ideal for decorative images. However, be careful not to abuse background images by using them for meaningful images without a text alternative.

#### Icon fonts

Icon fonts are a common way to include simple graphics, but they do not use `img` tags. Rather, they typically use CSS to insert their content via pseudo elements. Because they are not `img`s, developers often neglect text alternatives. Additionally, because some screen readers will read pseudo elements, use of icon fonts can cause unpronounceable characters to be spoken. To address these issues, when using icon fonts, developers should place the icon in its own element with the `aria-hidden` attribute set to true, and add a visually-hidden text alternative in a sibling element. 

```html
<span>
    <span class="my-icon" aria-hidden="true"></span>
    <span class="visually-hidden">Alt text</span>
<span>
```

Note that a text alternative may not be necessary if adjacent text suitably describes the icon:

```html
<a href="http://facebook.com">
    <span class="facebook-icon" aria-hidden="true"></span>
    Facebook
</a>
```

#### Images of text

As much as possible, avoid images of text, and use web fonts instead. (1.4.5)

#### Video

TODO

#### Other graphical content (charts, SVGs, CSS shapes)

Graphical content may also be included via HTML canvas elements, inline SVGs, and CSS shapes (basic graphics created by clever composition of CSS rules -  for example, arrows or hamburger menus). In all cases, developers should ask themselves whether the visual content adds meaning that a non-sighted user would benefit from. For example, a hamburger menu constructed using pure CSS may be recognizable to a sighted user, but a user of a screen reader would not be able to identify it without additional instruction. (1.1.1)

### Keyboard operability

Developers should take a couple steps to provide support for keyboard-only users:

1. Clickable elements also need to be focusable, so they can be reached with a keyboard
2. Clickable elements should respond to keyboard events as well as mouse events, so they can be invoked via keyboard
3. Any other mouse- or touch-based interaction (drag and drop, for example) should have a keyboard alternative
4. Focus states should be implemented as designed
5. Additional key commands should be recognized as required for specific UI patterns (see https://www.w3.org/TR/wai-aria-practices/)

In many cases, the first two requirements can be addressed via semantic markup. Does clicking the element in question trigger some sort of navigation? Use an `a` tag. Does it invoke an action on the same page? Use a `button` tag. Is the element a custom form control? Try using styled built-in form controls (see below for more info). Because these are all standard HTML elements, browsers will provide robust built-in keyboard support. (2.1.1)

### Zoomability

Users with impaired eyesight may utilize screen magnifiers or browser zoom to enlarge content. These days, most browsers support this without issue, but developers should still spot-check their work at 200% browser zoom. (1.4.4)

### Forms

#### Form tags and submit buttons

HTML forms can typically be submitted both explicitly - by clicking the default submit button - or implicitly - by hitting 'enter' within a form field. Most (all?) browsers support both mechanisms, and many users expect both to be available. 

The simplest way to accommodate this expectation is to ensure form fields are always contained within a `form` element, and use the `form`'s `submit` event to trigger any client-side processing (validation, AJAX submissions, etc). This event will fire for both implicit and explicit submission. (3.2.2)

When a submit button is present in the design, it should be coded using a `<input type="submit" value="Label" />` or `<button type="submit">Label</button>`; non submit-type buttons, styled `a` tags with attached click handlers, etc, should not be used.

Bonus: on mobile Safari (and possibly other mobile browsers), the on-screen keyboard's 'return' button will change its text to 'Go' when on a field inside a `form` element (provided the `action` attribute is specified).

#### Labels

All form fields should have a programatically-associated label. (2.4.6, 3.3.2)

For visible labels, this means using a `label` element, either containing the input in question, or associated with it through the `label`'s `for` attribute.

```html
<label for="field-1">Field 1</label>
<input type="text" name="field-1" id="field-1" />

<label>
    Field 2
    <input type="text" name="field-2" />
</label>
```

Using `label` in this manner allows AT identify the input in question using the specified label text. For example, when a screen-reader user focuses on the first field in the preceding example, the text 'Field 1' will typically be read.

Most browsers also have built-in behavior to translate clicks on a `label` into some action on their associated input. Text inputs and selects are typically focused; radio buttons and checkboxes are typically toggled. This functionality is particularly useful for radios and checkboxes, since the control's click target would otherwise be very small and difficult to reach for many users.

##### Hidden labels

Form fields will sometimes be designed without labels, but this doesn't mean labels are optional; rather, a label needs to be provided in a manner that is both accessible and hidden from view.

This can be accomplished using visually-hidden `label`s in the manner described above, or by using the `aria-label` or `aria-labelledby` attribute. 

```html
<label for="field-3" class="visually-hidden">Field 3</label>
<input type="text" name="field-3" id="field-3" />

<label>
    <span class="visually-hidden">Field 4</span>
    <input type="text" name="field-4" />
</label>

<input type="text" name="field-5" aria-label="Field 5" />

<span id="field-6-label" style="display: none">Field 6</span>
<input type="text" name="field-6" aria-labelledby="field-6-label" />
```

Note that, typically, elements which are styled with `display: none` or `visibility: hidden` will not be accessible. However, their content *will* be included in *another element's*  accessible name/description when referenced by its `aria-labelledby` or `aria-describedby` attribute (see https://www.paciellogroup.com/blog/2015/05/short-note-on-aria-labelledby-and-aria-describedby/).

Note also that the HTML5 `placeholder` attribute is *not* a label.

#### Identifying required fields

Required fields should be identified using the HTML5 `required` attribute. This allows AT to identify to the user that the field is required.

```html
<label for="field-7">Field 7</label>
<input type="text" name="field-7" id="field-7" required="required" />
```

Note that default browser validation/styling may need to be disabled or overridden when using this attribute.

#### Grouping

Related fields should be grouped inside a `fieldset` element, with a `legend` as its first child. This is particularly important for checkbox and radio button lists, which typically include both a prompt (the `legend`) for the group as a whole and labels for each individual item. (3.3.2)

```html
<fieldset>
    <legend>Group</legend>
    <input type="radio" name="field-8" id="field-8-option-1" value="option-1" /> <label for="field-8-option-1">Option 1</label>
    <input type="radio" name="field-8" id="field-8-option-2" value="option-2" /> <label for="field-8-option-2">Option 2</label>
    <input type="radio" name="field-8" id="field-8-option-3" value="option-3" /> <label for="field-8-option-3">Option 3</label>
</fieldset>
```

Note that some screen readers are a bit particular about how and when they will read `legend`s (see http://webaim.org/discussion/mail_thread?thread=6714 for a good discussion of the issues). This section may be updated.

#### Instructions and other related content

Instructional content may be associated with a form field through its `aria-describedby` attribute. This content then becomes part of the field's accessible description, which AT will make available to the user. (3.3.2)

```html
<label for="field-9">Field 9</label>
<input type="text" name="field-9" id="field-9" required="required" aria-describedby="field-9-description" pattern="^(\d{3})[ -.]?(\d{2})[ -.]?(\d{4})$" />
<div id="field-9-description">Please enter a 9-digit SSN with or without dashes or spaces</div>
```

An element can have multiple descriptions - just add additional IDs to its `aria-describedby`. Note that in some UA/ATs, it is necessary to specify a `tabindex` (typically `-1`) on the description elements if multiple descriptions are present.

#### Input types

HTML5 introduced a plethora of new input types for email, dates, telephone numbers, urls, etc. While UA/AT support can be a bit spotty, that's no reason not to take advantage of the additional semantics and behaviors of these new input types. 

#### Identifying errors

Invalid form fields should have the `aria-invalid` attribute set to `true` *after the field has been interacted with or the form has been submitted*. A description of the error or how to fix it (if known) should either be contained within the field's label, or be associated with the input via its `aria-describedby` attribute. (3.3.1)

If submission fails on either client- or server-side validation, a list of errors should be presented and focused, or focus should be shifted to the first invalid field in the form. In both cases, AT will respond to the change in focus by announcing the newly-focused element, thereby informing the user of the error state. And in the latter case, by focusing the first form field, we save the user the trouble of having to find the errored field within the form. (3.3.1, 3.3.3)

```html
<!-- before submission -->
<label for="field-10">Field 10</label>
<input type="text" name="field-10" id="field-10" required="required" />
```

```html
<!-- after failed submission -->
<label for="field-10">Field 10</label>
<input type="text" name="field-10" id="field-10" required="required" aria-invalid="true" aria-describedby="field-10-description" />
<div id="field-10-description">Error: Field 10 is required; please complete</div>
```

#### Multi-part labels

TODO

#### Disclosures and agreements

TODO

#### Custom controls

Developers have a couple options for creating custom form controls. As much as possible, it's recommended to utilize standard HTML5 controls and apply creative styling. The standard checkbox, radio, text, select, etc controls are well-supported by AT, offer a well-understood user experience for disabled users, and often have extensive built-in functionality that would be difficult to replicate. 

That said, if the built-in HTML5 controls are not sufficient, true custom controls may be used, provided they adhere to the [ARIA design patterns](https://www.w3.org/TR/wai-aria-practices/#aria_ex).

### Focus management

When navigating with a keyboard, focus determines which element will receive keyboard events (and, consequently, which element the user can interact with). For example, when a link has focus, a keyboard user may follow that link by clicking enter. Focus will be set automatically by the browser as a user tabs through elements, but it may also be set programmatically.

Typically, developers should only manipulate focus in response to a user action that *requires* a change in focus. For example, clicking a button to open a dialog *should* result in a change in focus, but focusing or typing into a text field generally *should not*. In both cases, the goal is the same - to keep the experience consistent and predictable. (3.2.1, 3.2.2)

#### When is it necessary to manage focus? 

1. When removing or hiding an element that has focus (this includes removing an element whose descendant has focus). If the focused element is removed or hidden, the browser will typically revert focus back to the document or browser window, causing the user to lose their place on the page. If the element in question was added in response to a user action (for example, a dialog), focus should be returned to the element that initiated the action (assuming it is still in the DOM). 
2. When adding or showing an element that is intended to occlude the rest of the UI. For example, a modal dialog or loading overlay. In this scenario, in addition to shifting focus to the new element or a descendent, event handlers should intercept focus/tab key events to prevent any other elements from receiving focus. 
3. When form submission errors, focus should be moved either to a list of the errors, or the first errored field in the form.
4. When implementing other specific UI patterns that require it (see https://www.w3.org/TR/wai-aria-practices/).

#### When is it recommended to manage focus?

1. When transitioning between screens in a single page application. This can make for a more fluid experience.
2. When expanding a collapsible content region, focus should be shifted to the region.

#### What else should we consider when managing focus?

Be sure only to trap focus when absolutely necessary, and when focus is trapped, be sure to provide instructions for how to release it. (2.1.2)

### Tab order

Much like the overall order of content within the DOM, tab/focus order should generally follow the visual presentation of the page, so that users can tab through in a natural order. In most cases, this can be accomplished simply by ordering elements properly within the source HTML; explicitly setting a >0 tabindex should be avoided. (2.4.3)

### ARIA roles, states, and properties

TODO

## What are some considerations for testing?

### Design validation

Ideally, QA will not need to do a formal validation of the WCAG design requirements. Rather, those requirements should be addressed in the design phase, and QA therefore can be responsible for general design validation.

### Functional testing

Functional testing should be performed on all of the following:

- At least 1 popular screen reader/browser. See [WebAIM's latest survey](http://webaim.org/projects/screenreadersurvey6/) for some suggestions.
- With keyboard only.
- At 200% browser zoom.

### Testing with actual users

If available, users with disabilities should be brought in periodically for testing.

## Other resources

- https://www.w3.org/WAI/intro/wcag
- https://www.w3.org/WAI/WCAG20/quickref/
- https://www.w3.org/TR/wai-aria-practices/
- https://w3c.github.io/aria-in-html/
- http://webaim.org/
- http://oaa-accessibility.org/
- http://a11yproject.com/
- https://www.marcozehe.de/
- http://www.karlgroves.com/
- http://www.deque.com/blog/
- https://www.paciellogroup.com/blog/
- http://www.weba11y.com/
- https://a11ywins.tumblr.com/
- https://www.wuhcag.com/wcag-checklist/
- http://www.nvaccess.org/
