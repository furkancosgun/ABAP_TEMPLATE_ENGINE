# ABAP Template Engine

This project provides a flexible and powerful template engine for ABAP environments. It supports dynamic template rendering with conditional logic, loops, and partial templates. The following examples illustrate how to use various features of the template engine.

## License
This project is licensed under the [MIT License](LICENSE). See the LICENSE file for details.

## Contents
- [Conditional Rendering Example](#conditional-rendering-example)
- [Loop and Conditional Rendering Example](#loop-and-conditional-rendering-example)
- [Repeating Content Example](#repeating-content-example)
- [Repeating Content with Context Example](#repeating-content-with-context-example)
- [Partial Templates Example](#partial-templates-example)

### Conditional Rendering Example
This example demonstrates how to use conditional logic within a template to show different outputs based on a boolean flag.

```abap
* Define a template with conditional rendering
  DATA(template) = |<%if is_deleted %><p> Is Deleted = True </p><%endif%>| &&
                   |<%ifn is_deleted %><p> Is Deleted = False </p><%endifn%>|.

* Create an instance of the template engine with the defined template
  DATA(lo_template) = zcl_template_engine=>create( template = template ).

* Define the context structure
  TYPES: BEGIN OF lty_context,
           is_deleted TYPE boolean,
         END OF lty_context.

* Initialize context with data
  DATA: ls_context TYPE lty_context VALUE 'X'.

* Render the template with the context
  cl_demo_output=>display( lo_template->render( ls_context ) ).
```
```html
<p> Is Deleted = True </p>
```

### Loop and Conditional Rendering Example
This example shows how to render a list of items, applying conditional display based on item properties.

```abap
* Define a template with a loop and conditional rendering
  DATA(template) = |<% for item in items %>| &&
                   |<% if item-show %>| &&
                   |<p>Item: <%item-name%></p>{ cl_abap_char_utilities=>newline }| &&
                   |<% endif %>| &&
                   |<% endfor %>|.

* Define the structure for loop items
  TYPES: BEGIN OF lty_context_line,
           name TYPE string,
           show TYPE boolean,
         END OF lty_context_line.

* Define the main context structure
  TYPES: BEGIN OF lty_context,
           items TYPE TABLE OF lty_context_line WITH EMPTY KEY,
           item  TYPE lty_context_line, "item required
         END OF lty_context.

* Initialize context with data
  DATA: ls_context TYPE lty_context.
  ls_context-items = VALUE #( ( name = 'Item 1' show = abap_true )
                              ( name = 'Item 2' show = abap_false )
                              ( name = 'Item 3' show = abap_true ) ).

* Create an instance of the template engine with the defined template
  DATA(lo_template) = zcl_template_engine=>create( template = template ).

* Render the template with the context
  cl_demo_output=>display( lo_template->render( ls_context ) ).
```
```html
<p>Item: Item 1</p>
<p>Item: Item 3</p>
```
### Repeating Content Example
This example demonstrates how to repeat a section of the template a specified number of times.

```abap
* Define a template to repeat content
  DATA(template) = |<% do 5 %><p>Hello World</p>{ cl_abap_char_utilities=>newline }<% enddo %>|.

* Create an instance of the template engine with the defined template
  DATA(lo_template) = zcl_template_engine=>create( template = template ).

* Render the template
  cl_demo_output=>display( lo_template->render( ) ).
```
```html
<p>Hello World</p>
<p>Hello World</p>
<p>Hello World</p>
<p>Hello World</p>
<p>Hello World</p>
```

### Repeating Content with Context Example
This example shows how to use a context variable to control the number of repetitions in the template.

```abap
* Define the context structure
  TYPES: BEGIN OF lty_context,
           count TYPE i,
         END OF lty_context.

* Initialize context with data
  DATA: ls_context TYPE lty_context.
  ls_context-count = 5.

* Define a template to repeat content based on context
  DATA(template) = |<% do count %><p>Hello World</p>{ cl_abap_char_utilities=>newline }<% enddo %>|.

* Create an instance of the template engine with the defined template
  DATA(lo_template) = zcl_template_engine=>create( template = template ).

* Render the template with the context
  cl_demo_output=>display( lo_template->render( ls_context ) ).
```
```html
<p>Hello World</p>
<p>Hello World</p>
<p>Hello World</p>
<p>Hello World</p>
<p>Hello World</p>
```
### Partial Templates Example
This example demonstrates how to use partial templates to modularize and reuse template snippets.

```abap
* Define partial templates with more descriptive content
  DATA(lo_partial1) = zcl_template_engine=>create(
      |Greetings from Partial 1! Here is the name provided in the context: <%name%>|
  ).
  DATA(lo_partial2) = zcl_template_engine=>create(
      |Hello from Partial 2! The name in the context is: <%name%>|
  ).
  DATA(lo_partial3) = zcl_template_engine=>create(
      |Welcome from Partial 3! Name accessed from context: <%name%>|
  ).

* Define a main template that uses the partial templates
  DATA(lo_template) = zcl_template_engine=>create(
      |<p>Message from Partial 1: <%@partial1%></p>{ cl_abap_char_utilities=>newline }| &&
      |<p>Message from Partial 2: <%@partial2%></p>{ cl_abap_char_utilities=>newline }| &&
      |<p>Message from Partial 3: <%@partial3%></p>{ cl_abap_char_utilities=>newline }| &&
      |<p>This is the main layout. The name used here is: <%name%></p>|
  ).

* Define the context structure including partial templates
  TYPES: BEGIN OF lty_context,
           partial1 TYPE REF TO zcl_template_engine,
           partial2 TYPE REF TO zcl_template_engine,
           partial3 TYPE REF TO zcl_template_engine,
           name     TYPE string,
         END OF lty_context.

* Initialize context with data
  DATA: ls_context TYPE lty_context.
  ls_context-partial1 = lo_partial1.
  ls_context-partial2 = lo_partial2.
  ls_context-partial3 = lo_partial3.
  ls_context-name     = 'TEMPLATE_ENGINE'.

* Render the main template with the context
  cl_demo_output=>display( lo_template->render( ls_context ) ).
```
```html
<p>Message from Partial 1: Greetings from Partial 1! Here is the name provided in the context: TEMPLATE_ENGINE</p>
<p>Message from Partial 2: Hello from Partial 2! The name in the context is: TEMPLATE_ENGINE</p>
<p>Message from Partial 3: Welcome from Partial 3! Name accessed from context: TEMPLATE_ENGINE</p>
<p>This is the main layout. The name used here is: TEMPLATE_ENGINE</p>
```

Feel free to adjust any details according to your specific needs or preferences.
