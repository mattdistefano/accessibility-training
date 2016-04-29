# Accessibility training

## What is accessibility? 

Accessibility in the digital realm refers to how well people with disabilities can access a digital property (in our case, web sites or apps). It can also refer to the overall practice or discipline of designing and developing *for* accessibility. 

## How people with disabilities interact with sites/apps?

Many users with disabilities will take advantage of some sort of assistive technology (AT). AT augments, adjusts, or even entirely replaces the screen/mouse paradigm of interaction. For example, a blind user may utilize a screen reader, which provides a spoken representation of the screen; a user with impaired motor skills might navigate via keyboard rather than mouse; a user with poor eyesight may zoom their browser. 

Note that not all disabilities require the use of AT. A user who is deaf or color-blind or cognitively-impaired may just browse with monitor and mouse, but still encounter content that is difficult to use (confusing language or videos without captions, for instance). 

Note too that AT is not limited to just desktops or laptops. Touch-based screen readers are available for tablets and smartphones.

## How does accessibility (or lack thereof) impact a disabled user's experience?

In the worst case, poor accessibility can render a site or app totally unusable. Critical content may be unreachable, and critical operations unavailable. 

## What are the relevant standards/guidelines?

WCAG 2.0 is the most widely-recognized guideline for digital accessibility. While it is not currently required by law, courts have identified it as a benchmark for accessibility in lawsuits brought under ADA, and it is likely to be the basis for future legislation. 

WCAG 2.0 is broken into 3 levels - A, AA, and AAA (AAA being the most stringent). We currently try to comply with AA.

## How do we comply?

WCAG compliance is a tricky subject. Because WCAG is intended to describe the requirements for accessibile digital applications *in general*, the details of meeting those requirements in a specific technology often require additional research and experimentation. Likewise, because WCAG often specifies outcomes instead of implementation details, real-world testing is frequently required. This in turn can be challenging, because UAs and ATs can themselves be buggy, different combinations of UA and AT may produce different results, AT can often be expensive and difficult to learn, etc. 

That said, there's a number of things we can do to promote accessibility in our projects:

1. Address accessibility early by including it in the visual and technical design process
2. Follow best practices and established patterns 
3. Use automation where viable to test for compliance
4. Perform basic manual testing during development and QA
5. Invite disabled users to participate in testing

## What are the considerations for design?

### Font size

TODO

### Colors and contrast

TODO

### IA and identification

TODO

### Content

TODO

### Text/audio alternatives

TODO

### Operation alternatives

TODO

## What are the considerations for development?

### Semantic markup

Developers should understand the meaning of different tags and choose them carefully. Remember: HTML is a language for adding structure and meaning to content - it's not just about putting that content on the screen. A surprisingly large portion of accessibility issues can be traced back to bad markup choices. In fact, much of this document concerns little more than proper use of HTML. Know your tags!

### Document structure

#### Headings

Headings should generally appear in order, where each level of heading represents a subheading within the previous heading's content.

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

#### Landmarks

Use specific tags and/or the `role` attribute to identify landmarks within a document. For example, the main content area should be marked up with a `main` tag so that supporting AT can skip directly to it.

### Hidden content

Throughout the examples in this document, the `visually-hidden` class will be used. This does not necessarily represent a _specific_ class, but is intended to represent the general concept of a style that hides content from the visual presentation while keeping it available to AT. In most cases, content that is styled with `display: none` or `visibility: hidden` will not be readable by AT. Often, this is what we want, but sometimes we want to keep content accessible, while still hiding it on the screen. In these cases, a `visually-hidden` style is appropriate. 

### Alternative content

Since not all users can see the visual presentation of a site or app, it's important to ensure that any meaningful information that is conveyed visually is also conveyed in an accessible, text-based manner. 

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

#### Video

TODO

#### Other graphical content (charts, SVGs, CSS shapes)

Images may also be included as CSS backgrounds, and CSS shapes may be used to construct basic graphics like arrows, hamburger menus, etc. In both cases, developers should ask themselves whether the visual content adds meaning that a non-sighted user would benefit from. For example, a hamburger menu constructed using pure CSS may be recognizable to a sighted user, but a screen reader would not understand it without additional instruction.

### Keyboard operability

Many disabled users rely exclusively on the keyboard for navigation. Therefore, any interactions or operations that are available through a mouse or touchscreen should also be available through a keyboard. This has a couple implications for development:

1. Any clickable element also needs to be focusable, so it can be reached with a keyboard
2. Any clickable element should respond to keyboard events as well as mouse events, so it can be invoked via keyboard
3. Any other mouse- or touch-based interaction (drag and drop, for example) should have a keyboard alternative

In many cases, the first two requirements can be addressed via semantic markup. Does clicking the element in question trigger some sort of navigation? Use an `a` tag. Does it invoke an action on the same page? Use a `button` tag. Is the element a custom form control? Try using styled built-in form controls (see below for more info). Because these are all standard HTML elements, browsers will provide robust built-in keyboard support. 

### Zoomability

Users with impaired eyesight may utilize screen magnifiers or browser zoom to enlarge content. These days, most browsers support this without issue, but developers should still spot-check their work at 200% browser zoom.

### Forms

#### Form tags and submit buttons

HTML forms can typically be submitted both explicitly - by clicking the default submit button - or implicitly - by hitting 'enter' within a form field. Most (all?) browsers support both mechanisms, and many users expect both to be available. 

The simplest way to accommodate this expectation is to ensure form fields are always contained within a `form` element, and use the `form`'s `submit` event to trigger any client-side processing (validation, AJAX submissions, etc). This event will fire for both implicit and explicit submission.

When a submit button is present in the design, it should be coded using a `<input type="submit" value="Label" />` or `<button type="submit">Label</button>`; non submit-type buttons, styled `a` tags with attached click handlers, etc, should not be used.

Bonus: on mobile Safari (and possibly other mobile browsers), the on-screen keyboard's 'return' button will change its text to 'Go' when on a field inside a `form` element (provided the `action` attribute is specified).

#### Labels

All form fields should have a programatically-associated label. 

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

This can be accomplished using the labeling pattern described above, with the label text visually-hidden (using for example foundation's `show-for-sr` class, or HTML5 BP's `visuallyhidden` class). It can also be accomplished using an `aria-label` or `aria-labelledby` attribute. 

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

#### Identifying required fields

Required fields should be identified using the HTML5 `required` attribute. This allows AT to identify to the user that the field is required.

```html
<label for="field-7">Field 7</label>
<input type="text" name="field-7" id="field-7" required="required" />
```

Note that default browser validation/styling may need to be disabled or overridden when using this attribute.

TODO: starring?

#### Grouping

Related fields should be grouped inside a `fieldset` element, with a `legend` as its first child. This is particularly important for checkbox and radio button lists, which typically include both a prompt (the `legend`) for the group as a whole and labels for each individual item.

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

Instructional content may be associated with a form field through its `aria-describedby` attribute. This content then becomes part of the field's accessible description, which AT will make available to the user.

```html
<label for="field-9">Field 9</label>
<input type="text" name="field-9" id="field-9" required="required" aria-describedby="field-9-description" pattern="^(\d{3})[ -.]?(\d{2})[ -.]?(\d{4})$" />
<div id="field-9-description">Please enter a 9-digit SSN with or without dashes or spaces</div>
```

An element can have multiple descriptions - just add additional IDs to its `aria-describedby`. Note that in some UA/ATs, it is necessary to specify a `tabindex` (typically `-1`) on the description elements if multiple descriptions are present.

TODO impact of `title` attribute here.

#### Input types

HTML5 introduced a plethora of new input types for email, dates, telephone numbers, urls, etc. While UA/AT support can be a bit spotty, that's no reason not to take advantage of the additional semantics and behaviors of these new input types. 

#### Identifying errors

Invalid form fields should have the `aria-invalid` attribute set to `true` *after the field has been interacted with or the form has been submitted*. 

If submission fails on either client- or server-side validation, either a list of errors should be presented and focused, or focus should be shifted to the first invalid field in the form. A description of the error or how to fix it (if known) should either be contained within the field's label, or be associated with the input via its `aria-describedby` attribute. 

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

### Focus management

When navigating with a keyboard, focus determines which element will receive keyboard events (and, consequently, which element the user can interact with). For example, when a link has focus, a keyboard user may follow that link by clicking enter. Focus will be set automatically by the browser as a user tabs through elements, but it may also be set programmatically.

Typically, developers should only manipulate focus in response to a user action that necessitates a change in focus. For example, clicking a button to open a dialog *should* result in a change in focus, but typing into a text field should not (unless the user is advised of this behavior in advance). 

#### When is it necessary to manage focus? 

1. When removing or hiding an element that has focus (this includes removing an element whose descendant has focus). If the focused element is removed or hidden, the browser will typically revert focus back to the document or browser window, causing the user to lose their place on the page. If the element in question was added in response to a user action (for example, a dialog), focus should be returned to the element that initiated the action (assuming it is still in the DOM). 
2. When adding or showing an element that is intended to occlude the rest of the UI. For example, a modal dialog or loading overlay. In this scenario, in addition to shifting focus to the new element or a descendent, event handlers should intercept focus/tab key events to prevent any other elements from receiving focus.
3. When form submission errors, focus should be moved either to a list of the errors, or the first errored field in the form.

#### When is it recommended to manage focus?

1. When transitioning between screens in a single page application. This can make for a more fluid experience.
2. When expanding a collapsible content region, focus should be shifted to the region.

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

https://www.w3.org/WAI/intro/wcag
http://webaim.org/
http://a11yproject.com/
https://www.marcozehe.de/
http://www.karlgroves.com/
http://www.deque.com/blog/
https://www.paciellogroup.com/blog/
http://www.weba11y.com/blog/
https://a11ywins.tumblr.com/
