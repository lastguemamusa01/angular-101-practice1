# angular-101-practice1

[Edit on StackBlitz ⚡️](https://stackblitz.com/edit/angular-101-practice1)

Angular is an application design framework and development platform for creating efficient 
and sophisticated single-page apps

need nodejs, npm package manager and angular cli installed

A component-based framework for building scalable web applications

ng build --prod   - to create the applicacion in the dist folder to deployment

key architectural elements

    - Binding syntax

      Data binding and HTML

        Developers can customize HTML by specifying attributes with string values. In the following example, class, src, and disabled modify the <div>, <img>, and <button> elements respectively

        <!-- Bind button disabled state to `isUnchanged` property -->
        <button [disabled]="isUnchanged">Save</button>

        Notice that the binding is to the disabled property of the button's DOM element, not the attribute. Data binding works with properties of DOM elements, components, and directives, not HTML attributes.
        
      HTML attributes and DOM properties

        Angular binding distinguishes between HTML attributes and DOM properties.

        Attributes initialize DOM properties and you can configure them to modify an element's behavior. Properties are features of DOM nodes.

          A few HTML attributes have 1:1 mapping to properties; for example, id.

          Some HTML attributes don't have corresponding properties; for example, aria-*.

          Some DOM properties don't have corresponding attributes; for example, textContent.

        In Angular, the only role of HTML attributes is to initialize element and directive state.

        When you write a data binding, you're dealing exclusively with the DOM properties and events of the target object.

        The HTML attribute value specifies the initial value; the DOM value property is the current value.

        <input [disabled]="condition ? true : false">.  -> property biding
        <input [attr.disabled]="condition ? 'disabled' : null">.   -> attribute binding.

        property is for dom and attirbute is for html

        Generally, use property binding over attribute binding as a boolean value is easy to read, the syntax is shorter, and a property is more performant.
      
      Type of data binding

        Angular provides three categories of data binding according to the direction of data flow:

          From the source to view
            Interpolation
            Property
              Element property
                <img [src]="heroImageUrl">
              Component property
                <app-hero-detail [hero]="currentHero"></app-hero-detail>
              Directive property
                <div [ngClass]="{'special': isSpecial}"></div>

            Attribute
              Attribute (the exception)
                <button [attr.aria-label]="help">help</button>
            Class
              class property
                <div [class.special]="isSpecial">Special</div>
            Style
              style property
                <button [style.color]="isSpecial ? 'red' : 'green'">

          From view to source
            Event
              Element event
                <button (click)="onSave()">Save</button>
              Component event
                <app-hero-detail (deleteRequest)="deleteHero()"></app-hero-detail>
              Directive event
                <div (myClick)="clicked=$event" clickable>click me</div>

          In a two way sequence of view to source to view
            Two-way
              Event and property
                <input [(ngModel)]="name">

          Use [] to bind from source to view.
          Use () to bind from view to source.
          Use [()] to bind in a two way sequence of view to source to view.

      How event binding workspace

        In an event binding, Angular configures an event handler for the target event. You can use event binding with your own custom events.

        When the component or directive raises the event, the handler executes the template statement. The template statement performs an action in response to the event.

        A common way to handle events is to pass the event object, $event, to the method handling the event. The $event object often contains information the method needs, such as a user's name or an image URL.

    - Template variables

      Template variables help you use data from one part of a template in another part of the template. With template variables, you can perform tasks such as respond to user input or finely tune your application's forms.

      A template variable can refer to the following:

        a DOM element within a template
        a directive
        an element
        TemplateRef
        a web component

      In the template, you use the hash symbol, #, to declare a template variable.

      <input #phone placeholder="phone number" />
      
      <!-- lots of other elements -->

      <!-- phone refers to the input element; pass its `value` to an event handler -->
      <button (click)="callPhone(phone.value)">Call</button>

      How Angular assigns values to template variables

        Angular assigns a template variable a value based on where you declare the variable:

          If you declare the variable on a component, the variable refers to the component instance.
          If you declare the variable on a standard HTML tag, the variable refers to the element.
          If you declare the variable on an <ng-template> element, the variable refers to a TemplateRef instance, which represents the template. For more information on <ng-template>, see How Angular uses the asterisk, *, syntax in Structural directives.
          If the variable specifies a name on the right-hand side, such as #var="ngModel", the variable refers to the directive or component on the element with a matching exportAs name.

      Using NgForm with template variables

        The NgForm directive demonstrates getting a reference to a different value by reference a directive's exportAs name.

        <form #itemForm="ngForm" (ngSubmit)="onSubmit(itemForm)">
          <label for="name">Name <input class="form-control" name="name" ngModel required />
          </label>
          <button type="submit">Submit</button>
        </form>

        <div [hidden]="!itemForm.form.valid">
          <p>{{ submitMessage }}</p>
        </div>

        Without the ngForm attribute value, the reference value of itemForm would be the HTMLFormElement, <form>. There is, however, a difference between a Component and a Directive in that Angular references a Component without specifying the attribute value, and a Directive does not change the implicit reference, or the element.

        With NgForm, itemForm is a reference to the NgForm directive with the ability to track the value and validity of every control in the form.

        Unlike the native <form> element, the NgForm directive has a form property. The NgForm form property allows you to disable the submit button if the itemForm.form.valid is invalid

      Template variable scope

        You can refer to a template variable anywhere within its surrounding template. Structural directives, such as *ngIf and *ngFor, or <ng-template> act as a template boundary. You cannot access template variables outside of these boundaries.

      Accessing in a nested template

        An inner template can access template variables that the outer template defines.

        In the following example, changing the text in the <input> changes the value in the <span> because Angular immediately updates changes through the template variable, ref1.

        <input #ref1 type="text" [(ngModel)]="firstExample" />
        <span *ngIf="true">Value: {{ ref1.value }}</span>

        <input #ref1 type="text" [(ngModel)]="firstExample" />

        <!-- New template -->
        <ng-template [ngIf]="true">
          <!-- Since the context is inherited, the value is available to the new template -->
          <span>Value: {{ ref1.value }}</span>
        </ng-template>


        <input *ngIf="true" #ref2 type="text" [(ngModel)]="secondExample" />
        <span>Value: {{ ref2?.value }}</span> <!-- doesn't work -->

        <ng-template [ngIf]="true">
          <!-- The reference is defined within a template -->
          <input #ref2 type="text" [(ngModel)]="secondExample" />
        </ng-template>
        <!-- ref2 accessed from outside that template doesn't work -->
        <span>Value: {{ ref2?.value }}</span>

        With structural directives, such as *ngFor or *ngIf, there is no way for Angular to know if a template is ever instantiated.

        As a result, Angular isn't able to access the value and returns an error.

      Accessing a template variable within <ng-template>

        When you declare the variable on an <ng-template>, the variable refers to a TemplateRef instance, which represents the template.

          <ng-template #ref3></ng-template>
          <button (click)="log(ref3)">Log type of #ref</button>

        In this example, clicking the button calls the log() function, which outputs the value of #ref3 to the console. Because the #ref variable is on an <ng-template>, the value is TemplateRef.

      Template input variable

        A template input variable is a variable you can reference within a single instance of the template. You declare a template input variable using the let keyword as in let hero.

        There are several such variables in this example: hero, i, and odd.

        The variable's scope is limited to a single instance of the repeated template. You can use the same variable name again in the definition of other structural directives.

        In contrast, you declare a template variable by prefixing the variable name with #, as in #var. A template variable refers to its attached element, component, or directive.

        Template input variables and template variables names have their own namespaces. The template input variable hero in let hero is distinct from the template variable hero in #hero

    - Components

        A component class that handles data and functionality.
        An HTML template that determines the UI.
        Component-specific styles that define the look and feel.

        An HTML template that declares what renders on the page
        A Typescript class that defines behavior
        A CSS selector that defines how the component is used in a template
        Optionally, CSS styles applied to the template

        Lifecycle hooks
          You can respond to events in the lifecycle of a component or directive by implementing one or more of the lifecycle hook interfaces in the Angular core library.

          - ngOnChanges()   - Respond when Angular sets or resets data-bound input properties. only call with inputs. each action in the input detect the every changes made in input.  it is very expensive
          - ngOnInit().     - Initialize the directive or component after Angular first displays the data-bound properties and sets the directive or component's input properties.
            You should not, for example, fetch data in a component constructor.
            ngOnInit() is a good place for a component to fetch its initial data.
            Constructors should do no more than set the initial local variables to simple values.
          
          - ngDoCheck().    - Detect and act upon changes that Angular can't or won't detect on its own. for custom changes.  it is very expensive

          The AfterContent hooks concern ContentChildren, the child components that Angular projected into the component.
          - ngAfterContentInit(). -  Respond after Angular projects external content into the component's view, or into the view that a directive is in
          - ngAfterContentChecked()	- Respond after Angular checks the content projected into the directive or component.
          
          The AfterView hooks concern ViewChildren, the child components whose element tags appear within the component's template.
          - ngAfterViewInit().     - Respond after Angular initializes the component's views and child views, or the view that contains the directive.
          - ngAfterViewChecked(). (dont use)  - Respond after Angular checks the component's views and child views, or the view that contains the directive. 
          
          - ngOnDestroy().     - Cleanup just before Angular destroys the directive or component. Unsubscribe Observables and detach event handlers to avoid memory leaks.
            Unsubscribe from Observables and DOM events.
            Stop interval timers.
            Unregister all callbacks that the directive registered with global or application services.
            ngOnDestroy() method is also the time to notify another part of the application that the component is going away.

        Component interaction
          Pass data from parent to child with input binding
            Intercept input property changes with a setter
            Intercept input property changes with ngOnChanges()
          Parent listens for child event
            The child component exposes an EventEmitter property with which it emits events when something happens
          Parent interacts with child via local variable. (can just access child component).  <app-countdown-timer #timer></app-countdown-timer>
          Parent calls an @ViewChild().  (can access to read and write child component).    @ViewChild(CountdownTimerComponent) and ngAfterViewInit()
          Parent and children communicate via a service
            subscription and unsubscribe()
        
        Component styles
          Using component styles
            styles: ['h1 { font-weight: normal; }']
            Class names and selectors are local to the component and don't collide with classes and selectors used elsewhere in the application.
          Special selectors
            :host - pseudo-class selector to target styles in the element that hosts the component (as opposed to targeting elements inside the component's template).
            :host-context - Sometimes it's useful to apply styles based on some condition outside of a component's view.
            :host-context() selector looks for a CSS class in any ancestor of the component host element, up to the document root

            :host-context(.theme-light) h2 {
              background-color: #eef;
            }
            applies a background-color style to all <h2> elements inside the component, only if some ancestor element has the CSS class theme-light.

            (deprecated) /deep/, >>>, and ::ng-deep
            Applying the ::ng-deep pseudo-class to any CSS rule completely disables view-encapsulation for that rule. Any style with ::ng-deep applied becomes a global style. In order to scope the specified style to the current component and all its descendants, be sure to include the :host selector before ::ng-deep
            :host ::ng-deep h3 { }. targets all <h3> elements, from the host element down through this component to all of its child elements in the DOM.

            Use /deep/, >>> and ::ng-deep only with emulated view encapsulation
          
          Loading component styles
            By setting styles or styleUrls metadata.
              styles: ['h1 { font-weight: normal; }']
              styleUrls: ['./hero-app.component.css']
            Inline in the template HTML.
              template: `
                        <style>
                          button {
                            background-color: white;
                            border: 1px solid #777;
                          }
                        </style>
                        <h3>Controls</h3>
                        <button (click)="activate()">Activate</button>
                      `
              or in inteplate use  <link rel="stylesheet" href="../assets/hero-team.component.css">
          With CSS imports.
            in css file, @import './hero-details-box.css';

          External and global style files
            When building with the CLI, you must configure the angular.json to include all external assets, including external style files.
            Register global style files in the styles section which, by default, is pre-configured with the global styles.css file.
          
          Non-CSS style files
            If you're building with the CLI, you can write style files in sass, less, or stylus
            styleUrls: ['./app.component.scss']
          
        Dynamic component loader
          - like add banner
          The ad banner uses a helper directive called AdDirective to mark valid insertion points in the template.
          AdDirective injects ViewContainerRef
          <ng-template adHost></ng-template>
          private componentFactoryResolver: componentFactoryResolver  

        Angular elements

          Angular elements are Angular components packaged as custom elements (also called Web Components), a web standard for defining new HTML elements in a framework-agnostic way.
          The @angular/elements package exports a createCustomElement() API that provides a bridge from Angular's component interface and change detection functionality to the built-in DOM API.
          Custom elements bootstrap themselves - they start automatically when they are added to the DOM, and are automatically destroyed when removed from the DOM.

    - Templates
        In Angular, a template is a chunk of HTML
        To eliminate the risk of script injection attacks, Angular does not support the <script> element in templates.

        - String interpolation is to show dynamic text of the property
          Text interpolation allows you to incorporate dynamic string values into your HTML templates.
          {{ product.name }}
          
          exressions with interpolation
            <!-- "The sum of 1 + 1 is 2" -->
            <p>The sum of 1 + 1 is {{1 + 1}}.</p>
            <p>The sum of 1 + 1 is not {{1 + 1 + getVal()}}.</p>

          Template expressions are similar to JavaScript

            You can't use JavaScript expressions that have or promote side effects, including:
              Assignments (=, +=, -=, ...)
              Operators such as new, typeof, or instanceof
              Chaining expressions with ; or ,
              The increment and decrement operators ++ and --
              Some of the ES2015+ operators

            Other notable differences from JavaScript syntax include:
              No support for the bitwise operators such as | and &
              New template expression operators, such as |, ?. and !

          Expression context

            Interpolated expressions have a context—a particular part of the application to which the expression belongs. Typically, this context is the component instance.
              <h4>{{recommended}}</h4>
              <img [src]="itemImageUrl2">

              <ul>
                <li *ngFor="let customer of customers">{{customer.name}}</li>
              </ul>

              This next example features a template reference variable, #customerInput.

              <label>Type something:
                <input #customerInput>{{customerInput.value}}
              </label>

          Expression best practices
            Use short expressions
            Quick execution
            No visible side effects
        
        Template statements

            Template statements are methods or properties that you can use in your HTML to respond to user events
            (event)="statement"
            <button (click)="deleteHero()">Delete hero</button>

            Syntax

            Like template expressions, template statements use a language that looks like JavaScript. However, the parser for template statements differs from the parser for template expressions.

            The following JavaScript and template expression syntax is not allowed:

              new
              increment and decrement operators, ++ and --
              operator assignment, such as += and -=
              the bitwise operators, such as | and &
              the pipe operator

            Statements have a context—a particular part of the application to which the statement belongs.
            
            #heroForm - reference variable

            <button (click)="onSave($event)">Save</button>
            <button *ngFor="let hero of heroes" (click)="deleteHero(hero)">{{hero.name}}</button>
            <form #heroForm (ngSubmit)="onSubmit(heroForm)"> ... </form>

            Statement best practices
              Conciseness
              Work within the context

        Transforming Data Using Pipes
          Use pipes to transform strings, currency amounts, dates, and other data for display

          Pipes are simple functions you can use in template expressions to accept an input value and return a transformed values

            DatePipe: Formats a date value according to locale rules.
            UpperCasePipe: Transforms text to all upper case.
            LowerCasePipe: Transforms text to all lower case.
            CurrencyPipe: Transforms a number to a currency string, formatted according to locale rules.
            DecimalPipe: Transforms a number into a string with a decimal point, formatted according to locale rules.
            PercentPipe: Transforms a number to a percentage string, formatted according to locale rules.

          You can also create pipes to encapsulate custom transformations and use your custom pipes in template expressions.

          <p>The hero's birthday is {{ birthday | date }}</p>
          The template expression {{ amount | currency:'EUR' }} transforms the amount to currency in euros

          You can chain pipes so that the output of one pipe becomes the input to the next.
          {{ birthday | date | uppercase}}

          By default, pipes are defined as pure so that Angular executes the pipe only when it detects a pure change to the input value. A pure change is either a change to a primitive input value (such as String, Number, Boolean, or Symbol), or a changed object reference (such as Date, Array, Function, or Object).

          One way to get the behavior you want is to change the object reference itself. You can replace the array with a new array containing the newly changed elements, and then input the new array to the pipe. In the above example, you can create an array with the new hero appended, and assign that to heroes. Angular detects the change in the array reference and executes the pipe.

          To summarize, if you mutate the input array, the pure pipe doesn't execute. If you replace the input array, the pipe executes and the display is updated,

          Detecting impure changes within composite objects

          To execute a custom pipe after a change within a composite object, such as a change to an element of an array, you need to define your pipe as impure to detect impure changes. Angular executes an impure pipe every time it detects a change with every keystroke or mouse movement.   A long-running impure pipe could dramatically slow down your app.

          @Pipe({
            name: 'flyingHeroesImpure',
            pure: false
          })

          Unwrapping data from an observable

            Observables let you pass messages between parts of your application. Observables are recommended for event handling, asynchronous programming, and handling multiple values. Observables can deliver single or multiple values of any type, either synchronously (as a function delivers a value to its caller) or asynchronously on a schedule.

            Use the built-in AsyncPipe to accept an observable as input and subscribe to the input automatically. Without this pipe, your component code would have to subscribe to the observable to consume its values, extract the resolved values, expose them for binding, and unsubscribe when the observable is destroyed in order to prevent memory leaks. AsyncPipe is an impure pipe that saves boilerplate code in your component to maintain the subscription and keep delivering values from that observable as they arrive.

            <p>Message: {{ message$ | async }}</p>

           Caching HTTP requests

            To communicate with backend services using HTTP, the HttpClient service uses observables and offers the HttpClient.get() method to fetch data from a server. The asynchronous method sends an HTTP request, and returns an observable that emits the requested data for the response.

            As shown in the previous section, you can use the impure AsyncPipe to accept an observable as input and subscribe to the input automatically. You can also create an impure pipe to make and cache an HTTP request.

            Impure pipes are called whenever change detection runs for a component, which could be every few milliseconds for CheckAlways. To avoid performance problems, call the server only when the requested URL changes, as shown in the following example, and use the pipe to cache the server response. The tabs show the following:

            Pipes and precedence

              The pipe operator has a higher precedence than the ternary operator (?:), which means a ? b : c | x is parsed as a ? b : (c | x). The pipe operator cannot be used without parentheses in the first and second operands of ?:.

        Property binding
            - Property bindings, to help you set values for properties and attributes of HTML elements and pass values to your application's presentation logic.
            <a [title]="product.name + ' details'">
    
            Property binding in Angular helps you set values for properties of HTML elements or Directives

            Property binding moves a value in one direction, from a component's property into a target element property.

            Binding to a property

              To bind to an element's property, enclose it in square brackets, [], which identifies the property as a target property. A target property is the DOM property to which you want to assign a value.
              <img [src]="itemImageUrl">

              colspan and colSpan
              A common point of confusion is between the attribute, colspan, and the property, colSpan. Notice that these two names differ by only a single letter.

            <!-- Notice the colSpan property is camel case -->
            <tr><td [colSpan]="1 + 1">Three-Four</td></tr>

            <!-- Bind button disabled state to `isUnchanged` property -->
            <button [disabled]="isUnchanged">Disabled Button</button>

            Setting a directive property
            <p [ngClass]="classes">[ngClass] binding to the classes property making this blue</p>

            Bind values between components using @input

            Property binding and security - sanitize and dont allow script tag

            Property binding and interpolation
            Often interpolation and property binding can achieve the same results. The following binding pairs do the same thing.

            <p><img src="{{itemImageUrl}}"> is the <i>interpolated</i> image.</p>
            <p><img [src]="itemImageUrl"> is the <i>property bound</i> image.</p>

            <p><span>"{{interpolationTitle}}" is the <i>interpolated</i> title.</span></p>
            <p>"<span [innerHTML]="propertyTitle"></span>" is the <i>property bound</i> title.</p>

        Attribute, class, and style bindings


            Binding to an attribute

              It is recommended that you set an element property with a property binding whenever possible. However, sometimes you don't have an element property to bind. In those situations, you can use attribute binding.

              Attribute binding in Angular helps you set values for attributes directly. With attribute binding, you can improve accessibility, style your application dynamically, and manage multiple CSS classes or styles simultaneously.

              <p [attr.attribute-you-are-targeting]="expression"></p>

              <!-- create and set an aria attribute for assistive technology -->
              <button [attr.aria-label]="actionName">{{actionName}} with Aria</button>

              <!--  expression calculates colspan=2 -->
              <tr><td [attr.colspan]="1 + 1">One-Two</td></tr>

            Binding to the class attribute

              you can use class binding to add and remove CSS class names from an element's class attribute.


              Binding to a single CSS class - To create a single class binding, use the prefix class followed by a dot and the name of the CSS class—for example, [class.sale]="onSale"

              Binding to multiple CSS classes - [class]="classExpression".   ->  "my-class-1 my-class-2 my-class-3"

            Binding to the style Attribute
              
              Binding to a single style - <nav [style.background-color]="expression"></nav>

              Binding to multiple styles - <nav [style]='navStyle'>. -> navStyle = 'font-size: 1.2rem; color: cornflowerblue;';

              The NgStyle directive can be used as an alternative to direct [style] bindings. ngStyle will be depricated.

            Styling Precedence

              A single HTML element can have its CSS class list and style values bound to multiple sources (for example, host bindings from multiple directives).

              Remember, use @Input() when you want to keep track of the attribute value and update the associated property. Use @Attribute() when you want to inject the value of an HTML attribute to a component or directive constructor.

        Event binding

          Event binding allows you to listen for and respond to user actions such as keystrokes, mouse movements, clicks, and touches.

          Binding to events

            <button (click)="onSave()">Save</button>

          Custom events with EventEmitter

            Directives typically raise custom events with an Angular EventEmitter as follows.  @Output

            Determining an event target
            <h4>myClick is an event on the custom ClickDirective:</h4>
            <button (myClick)="clickMessage=$event" clickable>click with myClick</button>
            {{clickMessage}}

        Two way binding

            Two-way binding gives components in your application a way to share data. Use two-way binding to listen for events and update values simultaneously between parent and child components.
            Two-way binding combines property binding with event binding
            <app-sizer [(size)]="fontSizePx"></app-sizer>
            use @input and @output

        SVG as templates
        
          You can use SVG files as templates in your Angular applications. When you use an SVG as the template, you are able to use directives and bindings just like with HTML templates. With these features, you can dynamically generate interactive graphics.

        Sharing data between child and parent directives and components

          <parent-component>
            <child-component></child-component>
          </parent-component>
          The <parent-component> serves as the context for the <child-component>.

          @Input() and @Output() give a child component a way to communicate with its parent component. @Input() allows a parent component to update data in the child component. Conversely, @Output() allows the child to send data to a parent component.

          Sending data to a child component

            The @Input() decorator in a child component or directive signifies that the property can receive its value from its parent component.
            To watch for changes on an @Input() property, you can use OnChanges, one of Angular's lifecycle hooks. 

          Sending data to a parent component

            The @Output() decorator in a child component or directive allows data to flow from the child to the parent.


          Using @Input() and @Output() together
            
            You can use @Input() and @Output() on the same child component as follows:

Directives - (Structural directives) *ngIf and *ngFor or custom directive

    You can use directives to perform a variety of tasks, such as dynamically modifying the DOM structure

    Directives are classes that add additional behavior to elements in your Angular applications. With Angular's built-in directives, you can manage forms, lists, styles, and what users see.

    The different types of Angular directives are as follows:
        Components—directives with a template. This type of directive is the most common directive type.
        Attribute directives—directives that change the appearance or behavior of an element, component, or another directive.
        Structural directives—directives that change the DOM layout by adding and removing DOM elements.

    Built-in attribute directives
      
      Attribute directives listen to and modify the behavior of other HTML elements, attributes, properties, and components.

        Many NgModules such as the RouterModule and the FormsModule define their own attribute directives. The most common attribute directives are as follows:
          NgClass—adds and removes a set of CSS classes.
          NgStyle—adds and removes a set of HTML styles.
          NgModel—adds two-way data binding to an HTML form element.

      
    Built-in structural directives

        NgIf—conditionally creates or disposes of subviews from the template.
        NgFor—repeat a node for each item in a list.
        NgSwitch—a set of directives that switch among alternative views.

      Getting the index of *ngFor

        <div *ngFor="let item of items; let i=index">{{i + 1}} - {{item.name}}</div>
      
      Tracking items with *ngFor trackBy

        By tracking changes to an item list, you can reduce the number of calls your application makes to the server. With the *ngFor trackBy property, Angular can change and re-render only those items that have changed, rather than reloading the entire list of items.

      Hosting a directive without a DOM element

        The Angular <ng-container> is a grouping element that doesn't interfere with styles or layout because Angular doesn't put it in the DOM.

        You can use <ng-container> when there's no single element to host the directive.

      Switching cases with NgSwitch
        
        Like the JavaScript switch statement, NgSwitch displays one element from among several possible elements, based on a switch condition. Angular puts only the 
        selected element into the DOM.

        NgSwitch—an attribute directive that changes the behavior of its companion directives.
        NgSwitchCase—structural directive that adds its element to the DOM when its bound value equals the switch value and removes its bound value when it doesn't equal the switch value.
        NgSwitchDefault—structural directive that adds its element to the DOM when there is no selected NgSwitchCase.
    
    Attribute directives
      
      With attribute directives, you can change the appearance or behavior of DOM elements and Angular components.

      ng generate directive highlight

      <p appHighlight>Highlight me!</p>

      Handling user events

        This section shows you how to detect when a user mouses into or out of the element and to respond by setting or clearing the highlight color.
        @HostListener()

      Passing values into an attribute directive

        @Input() property.
        @Input() appHighlight: string;

      Setting the value with user input

      Binding to a second property

      Deactivating Angular processing with NgNonBindable
        
        To prevent expression evaluation in the browser, add ngNonBindable to the host element. ngNonBindable deactivates interpolation, directives, and binding in templates.

        <p>Use ngNonBindable to stop evaluation.</p>
        <p ngNonBindable>This should not evaluate: {{ 1 + 1 }}</p>

    Writing structural directives

      This section guides you through creating an UnlessDirective and how to set condition values. The UnlessDirective does the opposite of NgIf, and condition values can be set to true or false. NgIf displays the template content when the condition is true. UnlessDirective displays the content when the condition is false.

        ng generate directive unless

        <p *appUnless="condition">Show this sentence unless the condition is true.</p>

        The UnlessDirective creates an embedded view from the Angular-generated <ng-template> and inserts that view in a view container adjacent to the directive's original <p> host element.

      Structural directive shorthand
      
        The asterisk, *, syntax on a structural directive, such as *ngIf, is shorthand that Angular interprets into a longer form. Angular transforms the asterisk in front of a structural directive into an <ng-template> that surrounds the host element and its descendants.

      Creating template fragments with <ng-template>

        Angular's <ng-template> element defines a template that doesn't render anything by default. With <ng-template>, you can render the content manually for full control over how the content displays.

      Shorthand	How Angular interprets the syntax
        *ngFor="let item of [1,2,3]"	<ng-template ngFor let-item [ngForOf]="[1,2,3]">

Dependency injection 

    allows you to declare the dependencies of your TypeScript classes without taking care of their instantiation. 
    Instead, Angular handles the instantiation for you. This design pattern allows you to write more 
    testable and flexible code.
    - example - using componenet declaring in the constructor the service class

    Dependencies are services or objects that a class needs to perform its function. Dependency injection, or DI, is a design pattern in which a class requests dependencies from external sources rather than creating them.

    Creating an injectable service

      ng generate service heroes/hero
      The metadata, providedIn: 'root', means that the HeroService is visible throughout the application.

    Injecting services

      Injecting services results in making them visible to a component.

      To inject a dependency in a component's constructor(), supply a constructor argument with the dependency type.
      constructor(heroService: HeroService)

    Using services in other services

      When a service depends on another service, follow the same pattern as injecting into a component. In the following example HeroService depends on a Logger service to report its activities.

    Dependency providers

      By configuring providers, you can make services available to the parts of your application that need them.
      providers: [Logger]

      Dependency injection tokens

        When you configure an injector with a provider, you are associating that provider with a dependency injection token, or DI token. The injector allows Angular create a map of any internal dependencies. The DI token acts as a key to that map.

      Specifying an alternative class provider
        [{ provide: Logger, useClass: BetterLogger }]

      Configuring class providers with dependencies
        [ UserService,
        { provide: Logger, useClass: EvenBetterLogger }]

      Aliasing class providers
        To alias a class provider, specify the alias and the class provider in the providers array with the useExisting property.

        [ NewLogger,
        // Alias OldLogger w/ reference to NewLogger
        { provide: OldLogger, useExisting: NewLogger}]
      
      Aliasing a class interface

        providers: [{ provide: Parent, useExisting: forwardRef(() => AlexComponent) }],

        To streamline your code, you can extract that logic into a helper function using the provideParent() helper function.

        // Helper method to provide the current component instance in the name of a `parentType`.
        export function provideParent
          (component: any) {
            return { provide: Parent, useExisting: forwardRef(() => component) };
          }

          providers:  [ provideParent(AliceComponent) ]

      Aliasing multiple class interfaces

        To alias multiple parent types, each with its own class interface token, configure provideParent() to accept more arguments.

        // Helper method to provide the current component instance in the name of a `parentType`.
        // The `parentType` defaults to `Parent` when omitting the second parameter.
        export function provideParent
          (component: any, parentType?: any) {
            return { provide: parentType || Parent, useExisting: forwardRef(() => component) };
          }

          providers:  [ provideParent(BethComponent, DifferentParent) ]

      Injecting an object

        To inject an object, configure the injector with the useValue option. The following provider object uses the useValue key to associate the variable with the Logger token.

        [{ provide: Logger, useValue: SilentLogger }]

        // An object in the shape of the logger service
        function silentLoggerFn() {}

        export const SilentLogger = {
          logs: ['Silent logger says "Shhhhh!". Provided via "useValue"'],
          log: silentLoggerFn
        };

      Injecting a configuration object

       common use case for object literals is a configuration object. The following configuration object includes the title of the application and the address of a web API endpoint.

       export const HERO_DI_CONFIG: AppConfig = {
          apiEndpoint: 'api.heroes.com',
          title: 'Dependency Injection'
        };

        To provide and inject the configuration object, specify the object in the @NgModule() providers array.

        src/app/app.module.ts (providers)

        providers: [
          UserService,
          { provide: APP_CONFIG, useValue: HERO_DI_CONFIG }
        ],

      Using an InjectionToken object

        you can define and use an InjectionToken object for choosing a provider token for non-class dependencies. The following example defines a token, APP_CONFIG of the type InjectionToken.

        src/app/app.config.ts

        import { InjectionToken } from '@angular/core';
        export const APP_CONFIG = new InjectionToken<AppConfig>('app.config');

        src/app/providers.component.ts
        
        providers: [{ provide: APP_CONFIG, useValue: HERO_DI_CONFIG }]
        Now you can inject the configuration object into the constructor with @Inject() parameter decorator.

        src/app/app.component.ts
        
        constructor(@Inject(APP_CONFIG) config: AppConfig) {
          this.title = config.title;
        }
      
      Interfaces and dependency Injection

        Though the TypeScript AppConfig interface supports typing within the class, the AppConfig interface plays no role in dependency injection. In TypeScript, an interface is a design-time artifact, and doesn't have a runtime representation, or token, that the DI framework can use.

        When the transpiler changes TypeScript to JavaScript, the interface disappears because JavaScript doesn't have interfaces.

        Since there is no interface for Angular to find at runtime, the interface cannot be a token, nor can you inject it.

        // Can't use interface as provider token
        [{ provide: AppConfig, useValue: HERO_DI_CONFIG })]

        // Can't inject using the interface as the parameter type
        constructor(private config: AppConfig){ }

      Using factory providers

        To create a changeable, dependent value based on information unavailable before run time, you can use a factory provider.

        only authorized users should see secret heroes in the HeroService. Authorization can change during the course of a single application session, as when a different user logs in .

        



First-party libraries

    Angular Router  
        [RouterLink](directive),  ActivatedRoute -> to get the value of the url link

    Angular Forms
        -ReactiveFormModule
            Import the FormBuilder service from the @angular/forms package. This service provides convenient methods 
            for generating controls.
            property Binding in form html tag [formGroup] and event biding (ngSubmit)

    Angular HttpClient
        is a built-in way to fetch data from external APIs and provide them to your application as a stream.
        Servers often return data in the form of a stream. Streams are useful because they make it easy to transform
        the returned data and make modifications to the way you request that data.
        - HttpClientModule need to be imported in app.module and is injected in the constructor of the service
        
    Angular Animations
    Angular PWA
    Angular Schematics

decorator 

    @Component() also provides metadata about the component, including its selector, templates, and styles.
    Pass data to a child component
        The @Input()(property binding) decorator indicates that the property value passes in from the component's parent
    Pass data to a parent component
        @Output()(event binding) allows the ProductAlertsComponent to emit an event when the value of the notify property changes.

Pipes

    A pipe is a way you can transform data in your HTML template
    currency pipe to transform product.price from a number to a currency string.
        product.price | currency

Service

    a service is an instance of a class that you can make available to any part of your application using Angular's
    dependency injection system.
    the service is injected in the constructor
    service that using httpCliente, The async pipe returns the latest value from a stream of data and continues 
    to do so for the life of a given component. When Angular destroys that component, the async pipe automatically stops

Angular cli
  
  
  -is the fastest, easiest, and recommended way to develop Angular applications

    install angular cli
    npm install -g @angular/cli

    help - with available commands
    ng help
    ng generate --help

    create new  workspace
    ng new my-first-project
    cd my-first-project
    ng serve

    add support for an external library to your project
    ng add @angular/pwa

    Compiles an Angular app into an output directory named dist/ at the given output path. Must be executed from within a workspace directory.
    ng build <project>

    Invokes the deploy builder for a specified project or for the default project in the workspace.
    ng deploy <project> [options]

    Opens the official Angular documentation (angular.io) in a browser, and searches for a given keyword.
    ng doc <keyword> 

    Builds and serves an Angular app, then runs end-to-end tests using Protractor.
    ng e2e <project> [options]

    Generates and/or modifies files based on a schematic.
    ng g - shortened
    ng generate <schematic>
      app-shell
      application
      class
      component
      directive
      enum
      guard
      interceptor
      interface
      library
      module
      pipe
      resolver
      service
      service-worker
      web-worker


    Runs linting tools on Angular app code in a given project folder.
    ng lint <project> 

    Runs an Architect target with an optional custom builder configuration defined in your project.
    ng run <target> 
    The CLI commands run Architect targets such as build, serve, test, and lint

    Builds and serves your app, rebuilding on file changes.
    ng serve <project> 

    Runs unit tests in a project.
    ng test <project>

    Updates your application and its dependencies.
    ng update

    Outputs Angular CLI version.
    ng version 




View encapsulation

    component CSS styles are encapsulated into the component's view and don't affect the rest of the application.

security

    best practice
      Keep current with the latest Angular library releases
      Don't modify your copy of Angular. 
      Avoid Angular APIs marked in the documentation as “Security Risk.”

    Preventing cross-site scripting (XSS)

      Cross-site scripting (XSS) enables attackers to inject malicious code into web pages
    
    Sanitization and security contexts

      Sanitization is the inspection of an untrusted value, turning it into a value that's safe to insert into the DOM. In many cases, sanitization doesn't change a value at all. Sanitization depends on context: a value that's harmless in CSS is potentially dangerous in a URL.
    
      For cases where this is unavoidable, use the built-in Angular sanitization functions. Sanitize untrusted values with the DomSanitizer.sanitize method and the appropriate SecurityContext


    Trusting safe values

      To mark a value as trusted, inject DomSanitizer and call one of the following methods:

        bypassSecurityTrustHtml
        bypassSecurityTrustScript
        bypassSecurityTrustStyle
        bypassSecurityTrustUrl
        bypassSecurityTrustResourceUrl

    Content security policy
        
        Content Security Policy (CSP) is a defense-in-depth technique to prevent XSS. To enable CSP, configure your web server to return an appropriate Content-Security-Policy HTTP header

    Enforcing Trusted Types

      We recommend the use of Trusted Types as a way to help secure your applications from cross-site scripting attacks. Trusted Types is a web platform feature that can help you prevent cross-site scripting attacks by enforcing safer coding practices. Trusted Types can also help simplify the auditing of application code.

    Use the AOT template compiler

      The AOT template compiler prevents a whole class of vulnerabilities called template injection, and greatly improves application performance. The AOT template compiler is the default compiler used by Angular CLI applications, and you should use it in all production deployments.


    Server-side XSS protection
      HTML constructed on the server is vulnerable to injection attacks. Injecting template code into an Angular application is the same as injecting executable code into the application: it gives the attacker full control over the application. To prevent this, use a templating language that automatically escapes values to prevent XSS vulnerabilities on the server. Don't generate Angular templates on the server side using a templating language; doing this carries a high risk of introducing template-injection vulnerabilities.
    HTTP-level vulnerabilities
      Angular has built-in support to help prevent two common HTTP vulnerabilities, cross-site request forgery (CSRF or XSRF) and cross-site script inclusion (XSSI). Both of these must be mitigated primarily on the server side, but Angular provides helpers to make integration on the client side easier.

    Cross-site request forgery
      In a cross-site request forgery (CSRF or XSRF), an attacker tricks the user into visiting a different web page (such as evil.com) with malignant code that secretly sends a malicious request to the application's web server (such as example-bank.com).

      In a common anti-XSRF technique, the application server sends a randomly generated authentication token in a cookie. The client code reads the cookie and adds a custom request header with the token in all subsequent requests. The server compares the received cookie value to the request header value and rejects the request if the values are missing or don't match.

      This technique is effective because all browsers implement the same origin policy. Only code from the website on which cookies are set can read the cookies from that site and set custom headers on requests to that site. That means only your application can read this cookie token and set the custom header. The malicious code on evil.com can't.

      Angular's HttpClient has built-in support for the client-side half of this technique. Read about it more in the HttpClient guide.

    Cross-site script inclusion (XSSI)

      Cross-site script inclusion, also known as JSON vulnerability, can allow an attacker's website to read data from a JSON API. The attack works on older browsers by overriding native JavaScript object constructors, and then including an API URL using a <script> tag.

      This attack is only successful if the returned JSON is executable as JavaScript. Servers can prevent an attack by prefixing all JSON responses to make them non-executable, by convention, using the well-known string ")]}',\n".

      Angular's HttpClient library recognizes this convention and automatically strips the string ")]}',\n" from all responses before further parsing.

Accessibility in Angular

    Building accessible web experience often involves setting ARIA attributes to provide semantic meaning where it might otherwise be missing

    The Angular Material library, which is maintained by the Angular team, is a suite of reusable UI components that aims to be fully accessible


Using lightweight injection tokens for libraries

The lightweight injection token design pattern consists of using a small abstract class as an injection token, and providing the actual implementation at a later stage. The abstract class is retained (not tree-shaken), but it is small and has no material impact on the application size.


deployment

  Simple deployment options
    Before fully deploying your application, you can test the process, build configuration, and deployed behavior by using one of these interim techniques.

  Building and serving from disk
    ng build --watch

  Automatic deployment with the CLI
    The Angular CLI command ng deploy (introduced in version 8.3.0) executes the deploy CLI builder associated with your project. A number of third-party builders implement deployment capabilities to different platforms. You can add any of them to your project by running ng add [package name].

  Basic deployment to a remote server
    Start with the production build: ng build --prod
    Copy everything within the output folder (dist/project-name/ by default) to a folder on the server. 
    Configure the server to redirect requests for missing files to index.html. Learn more about server-side redirects below.

  Deploy to GitHub Pages
    Server configuration

  Requesting services from a different server (CORS)
    Angular developers may encounter a cross-origin resource sharing error when making a service request (typically a data service request) to a server other than the application's own host server. Browsers forbid such requests unless the server permits them explicitly.

    There isn't anything the client application can do about these errors. The server must be configured to accept the application's requests.
  
  Lazy loading
    You can dramatically reduce launch time by only loading the application modules that absolutely must be present when the app starts.

    Configure the Angular Router to defer loading of all other modules (and their associated code), either by waiting until the app has launched or by lazy loading them on demand.

  Measure performance

    The Chrome DevTools Network Performance page is a good place to start learning about measuring performance.

    The WebPageTest tool is another good choice that can also help verify that your deployment was successful.

  Differential builders

    When you deploy using the Angular CLI build process, you can choose how and when to support differential loading. The ng build CLI command queries the browser configuration and the configured build target to determine if support for legacy browsers is required, and whether the build should produce the necessary bundles used for differential loading.

    The following configurations determine your requirements.

    Browserslist

      The Browserslist configuration file is included in your application project structure and provides the minimum browsers your application supports. See the Browserslist spec for complete configuration options.

    TypeScript configuration

      In the TypeScript configuration file, the "target" option in the compilerOptions section determines the ECMAScript target version that the code is compiled to. Modern browsers support ES2015 natively, while ES5 is more commonly used to support legacy browsers.

  Local development in older browsers

    Differential loading is not enabled by default for application projects that were generated with Angular CLI 10 and above. The ng serve, ng test, and ng e2e commands, however, generate a single ES2015 build which cannot run in older browsers that don't support the modules, such as IE 11.

    To maintain the benefits of differential loading, however, a better option is to define multiple configurations for ng serve, ng e2e, and ng test.


  Configuring browser compatibility

    The CLI uses Autoprefixer to ensure compatibility with different browser and browser versions. You may find it necessary to target specific browsers or exclude certain browser versions from your build.

    Internally, Autoprefixer relies on a library called Browserslist to figure out which browsers to support with prefixing.

  Configure environment-specific defaults
  Configuring size budgets
  Proxying to a backend server
    Rewrite the URL path
    Proxy multiple entries
    Bypass the proxy

  The CLI Builder API provides a new way of changing the behavior of the Angular CLI by using builders to execute custom logic.

  Server-side rendering (SSR) with Angular Universal

    This guide describes Angular Universal, a technology that renders Angular applications on the server.

    A normal Angular application executes in the browser, rendering pages in the DOM in response to user actions. Angular Universal executes on the server, generating static application pages that later get bootstrapped on the client. This means that the application generally renders more quickly, giving users a chance to view the application layout before it becomes fully interactive.

    There are three main reasons to create a Universal version of your app.

    Facilitate web crawlers through search engine optimization (SEO)
    Improve performance on mobile and low-powered devices
    Show the first page quickly with a first-contentful paint (FCP)

  Angular Language Service

    The Angular Language Service provides code editors with a way to get completions, errors, hints, and navigation inside Angular templates. It works with external templates in separate HTML files, and also with in-line templates.

    Angular Language Service is currently available as an extension for Visual Studio Code, WebStorm, Sublime Text and Eclipse IDE.

  Deprecated APIs and features
    
    dev api
      async	-> waitForAsync
    
    test api
      TestBed.get	-> TestBed.inject
      async	-> waitForAsync
    
    forms
      ngModel with reactive forms -> 	FormControlDirective
      FormBuilder.group legacy options parameter -> 	AbstractControlOptions parameter value
    
    
    /deep/, >>> and :ng-deep component style selectors
    Web Tracing Framework integration
    <template> tag. -> <ng-template>

    ngModel with reactive forms
    <input [formControl]="control" [(ngModel)]="value">

      use reactive forms
      use template-driven forms
    
    ReflectiveInjector

  Angular Ivy

    Ivy is the code name for Angular's next-generation compilation and rendering pipeline. With the version 9 release of Angular, the new compiler and runtime instructions are used by default instead of the older compiler and runtime, known as View Engine.

  Configuring application environments

    You can define different named build configurations for your project, such as stage and production, with different defaults.

    Each named configuration can have defaults for any of the options that apply to the various builder targets, such as build, serve, and test.

    A project's src/environments/ folder contains the base configuration file, environment.ts, which provides a default environment. You can add override defaults for additional environments, such as production and staging, in target-specific configuration files.

    myProject/src/environments/
                   └──environment.ts
                   └──environment.prod.ts
                   └──environment.stage.ts



    environment.ts
    export const environment = {
      production: false,
      apiUrl: 'http://my-api-url'
    };

    environment.prod.ts
    export const environment = {
      production: true,
      apiUrl: 'http://my-prod-url'
    };

    To use the environment configurations you have defined, your components must import the original environments file:

    app.component.ts
    import { environment } from './../environments/environment';

    The main CLI configuration file, angular.json, contains a fileReplacements section in the configuration for each build target, which allows you to replace any file in the TypeScript program with a target-specific version of that file.


    "configurations": {
      "production": {
        "fileReplacements": [
          {
            "replace": "src/environments/environment.ts",
            "with": "src/environments/environment.prod.ts"
          }
        ],


    ng build --prod or ng build --configuration=production
    

    You can also configure the serve command to use the targeted build configuration if you add it to the "serve:configurations" section of angular.json:

    "serve": {
      "builder": "@angular-devkit/build-angular:dev-server",
      "options": {
        "browserTarget": "your-project-name:build"
      },
      "configurations": {
        "production": {
          "browserTarget": "your-project-name:build:production"
        },
        "staging": {
          "browserTarget": "your-project-name:build:staging"
        }
      }
    },

    Configuring size budgets

      As applications grow in functionality, they also grow in size. The CLI allows you to set size thresholds in your configuration to ensure that parts of your application stay within size boundaries that you define.

      Define your size boundaries in the CLI configuration file, angular.json, in a budgets section for each configured environment.

    Configuring CommonJS dependencies

      It is recommended that you avoid depending on CommonJS modules in your Angular applications. Depending on CommonJS modules can prevent bundlers and minifiers from optimizing your application, which results in larger bundle sizes. Instead, it is recommended that you use ECMAScript modules in your entire application. For more information, see How CommonJS is making your bundles larger.

      The Angular CLI outputs warnings if it detects that your browser application depends on CommonJS modules. To disable these warnings, you can add the CommonJS module name to allowedCommonJsDependencies option in the build options located in angular.json file.

    Configuring browser compatibility

      The CLI uses Autoprefixer to ensure compatibility with different browser and browser versions. You may find it necessary to target specific browsers or exclude certain browser versions from your build.

      Internally, Autoprefixer relies on a library called Browserslist to figure out which browsers to support with prefixing. Browserlist looks for configuration options in a browserslist property of the package configuration file, or in a configuration file named .browserslistrc. Autoprefixer looks for the browserslist configuration when it prefixes your CSS.

      You can tell Autoprefixer what browsers to target by adding a browserslist property to the package configuration file, package.json:

      Alternatively, you can add a new file, .browserslistrc, to the project directory, that specifies browsers you want to support:

      Backward compatibility with Lighthouse

        If you want to produce a progressive web app and are using Lighthouse to grade the project, add the following browserslist entry to your package.json file, in order to eliminate the old flexbox prefixes:

      Backward compatibility with CSS grid
        
        CSS grid layout support in Autoprefixer, which was previously on by default, is off by default in Angular 8 and higher.

        To use CSS grid with IE10/11, you must explicitly enable it using the autoplace option. To do this, add the following to the top of the global styles file (or within a specific css selector scope):

      Proxying to a backend server

        You can use the proxying support in the webpack dev server to divert certain URLs to a backend server, by passing a file to the --proxy-config build option. For example, to divert all calls for http://localhost:4200/api to a server running on http://localhost:3000/api

        Create a file proxy.conf.json in your project's src/ folder.
        {
          "/api": {
            "target": "http://localhost:3000",
            "secure": false
          }
        }

        In the CLI configuration file, angular.json, add the proxyConfig option to the serve target:

        ...
        "architect": {
          "serve": {
            "builder": "@angular-devkit/build-angular:dev-server",
            "options": {
              "browserTarget": "your-application-name:build",
              "proxyConfig": "src/proxy.conf.json"
            },
        ...
        To run the dev server with this proxy configuration, call ng serve.

      Rewrite the URL path

        The pathRewrite proxy configuration option lets you rewrite the URL path at run time. For example, you can specify the following pathRewrite value to the proxy configuration to remove "api" from the end of a path.

        {
        "/api": {
          "target": "http://localhost:3000",
          "secure": false,
          "pathRewrite": {
            "^/api": ""
          }
        }
      }

      If you need to access a backend that is not on localhost, set the changeOrigin option as well. For example:

      To help determine whether your proxy is working as intended, set the logLevel option. For example:

    Proxy multiple entries

      You can proxy multiple entries to the same target by defining the configuration in JavaScript.

      Set the proxy configuration file to proxy.conf.js (instead of proxy.conf.json), and specify configuration files as in the following example.

      const PROXY_CONFIG = [
          {
              context: [
                  "/my",
                  "/many",
                  "/endpoints",
                  "/i",
                  "/need",
                  "/to",
                  "/proxy"
              ],
              target: "http://localhost:3000",
              secure: false
          }
      ]

      module.exports = PROXY_CONFIG;

      angular.JSON
      ...
      "architect": {
        "serve": {
          "builder": "@angular-devkit/build-angular:dev-server",
          "options": {
            "browserTarget": "your-application-name:build",
            "proxyConfig": "src/proxy.conf.js"
          },

    Bypass the proxy

      If you need to optionally bypass the proxy, or dynamically change the request before it's sent, add the bypass option, as shown in this JavaScript example.

      const PROXY_CONFIG = {
          "/api/proxy": {
              "target": "http://localhost:3000",
              "secure": false,
              "bypass": function (req, res, proxyOptions) {
                  if (req.headers.accept.indexOf("html") !== -1) {
                      console.log("Skipping proxy for browser request.");
                      return "/index.html";
                  }
                  req.headers["X-Custom-Header"] = "yes";
              }
          }
      }

      module.exports = PROXY_CONFIG;

    Using corporate proxy

      If you work behind a corporate proxy, the backend cannot directly proxy calls to any URL outside your local network. In this case, you can configure the backend proxy to redirect calls through your corporate proxy using an agent:



Workspace and project file structure

  You develop applications in the context of an Angular workspace. A workspace contains the files for one or more projects. A project is the set of files that comprise a standalone application or a shareable library.

  Workspace configuration files
  
    .editorconfig	Configuration for code editors. See EditorConfig.
    .gitignore	Specifies intentionally untracked files that Git should ignore.
    README.md	Introductory documentation for the root app.
    angular.json	CLI configuration defaults for all projects in the workspace, including configuration options for build, serve, and test tools that the CLI uses, such as TSLint, Karma, and Protractor. For details, see Angular Workspace Configuration.
    package.json	Configures npm package dependencies that are available to all projects in the workspace. See npm documentation for the specific format and contents of this file.
    package-lock.json	Provides version information for all packages installed into node_modules by the npm client. See npm documentation for details. If you use the yarn client, this file will be yarn.lock instead.
    src/	Source files for the root-level application project.
    node_modules/	Provides npm packages to the entire workspace. Workspace-wide node_modules dependencies are visible to all projects.
    tsconfig.json	The base TypeScript configuration for projects in the workspace. All other configuration files inherit from this base file. For more information, see the Configuration inheritance with extends section of the TypeScript documentation.
    tslint.json	Default TSLint configuration for projects in the workspace.

    angular.json

      version: The configuration-file version.
      newProjectRoot: Path where new projects are created. Absolute or relative to the workspace folder.
      defaultProject: Default project name to use in commands, where not provided as an argument. When you use ng new to create a new app in a new workspace, that app is the default project for the workspace until you change it here.
      schematics : A set of schematics that customize the ng generate sub-command option defaults for this workspace. See Generation schematics below.
      projects : Contains a subsection for each project (library or application) in the workspace, with the per-project configuration options.

      Strict mode

        When you create new workspaces and projects, you have the option to use Angular's strict mode, which can help you write better, more maintainable code. For more information, see Strict mode.

      Project configuration options
    
        root	The root folder for this project's files, relative to the workspace folder. Empty for the initial app, which resides at the top level of the workspace.
        sourceRoot	The root folder for this project's source files.
        projectType	One of "application" or "library". An application can run independently in a browser, while a library cannot.
        prefix	A string that Angular prepends to generated selectors. Can be customized to identify an app or feature area.
        schematics	A set of schematics that customize the ng generate sub-command option defaults for this project. See Generation schematics below.
        architect	Configuration defaults for Architect builder targets for this project.

        Generation schematics

          Angular generation schematics are instructions for modifying a project by adding files or modifying existing files. Individual schematics for the default Angular CLI ng generate sub-commands are collected in the package @schematics/angular. Specify the schematic name for a subcommand in the format schematic-package:schematic-name; for example, the schematic for generating a component is @schematics/angular:component.

        Project tool configuration options

          Architect is the tool that the CLI uses to perform complex tasks, such as compilation and test running. Architect is a shell that runs a specified builder to perform a given task, according to a target configuration. You can define and configure new builders and targets to extend the CLI. See Angular CLI Builders.

          Default Architect builders and targets

            app-shell
            browser
            dev-server
            extract-i18n
            karma
            protractor
            server
            tslint

          "architect": {
            "build": { },
            "serve": { },
            "e2e" : { },
            "test": { },
            "lint": { },
            "extract-i18n": { },
            "server": { },
            "app-shell": { }
          }


          The architect/build section configures defaults for options of the ng build command. See Build target below for more information.

          The architect/serve section overrides build defaults and supplies additional serve defaults for the ng serve command. In addition to the options available for the ng build command, it adds options related to serving the app.

          The architect/e2e section overrides build-option defaults for building end-to-end testing apps using the ng e2e command.

          The architect/test section overrides build-option defaults for test builds and supplies additional test-running defaults for the ng test command.

          The architect/lint section configures defaults for options of the ng lint command, which performs code analysis on project source files. The default linting tool for Angular is TSLint.

          The architect/extract-i18n section configures defaults for options of the ng extract-i18n command, which extracts marked message strings from source code and outputs translation files.

          The architect/server section configures defaults for creating a Universal app with server-side rendering, using the ng run <project>:server command.

          The architect/app-shell section configures defaults for creating an app shell for a progressive web app (PWA), using the ng run <project>:app-shell command.

        Build target

          builder	The npm package for the build tool used to create this target. The default builder for an application (ng build myApp) is @angular-devkit/build-angular:browser, which uses the webpack package bundler. Note that a different builder is used for building a library (ng build myLib).
          options	This section contains default build target options, used when no named alternative configuration is specified. See Default build targets below.
          configurations	This section defines and names alternative configurations for different intended destinations. It contains a section for each named configuration, which sets the default options for that intended environment. See Alternate build configurations below.

        Alternate build configurations

          You can define and name additional alternate configurations (such as stage, for instance) appropriate to your development process. Some examples of different build configurations are stable, archive and next used by AIO itself, and the individual locale-specific configurations required for building localized versions of an app. For details, see Internationalization (i18n).

        Additional build and test options

          The configurable options for a default or targeted build generally correspond to the options available for the ng build, ng serve, and ng test commands. For details of those options and their possible values, see the CLI Reference.

          assets	An object containing paths to static assets to add to the global context of the project. The default paths point to the project's icon file and its assets folder. See more in Assets configuration below.
          styles	An array of style files to add to the global context of the project. Angular CLI supports CSS imports and all major CSS preprocessors: sass/scss, less, and stylus. See more in Styles and scripts configuration below.
          stylePreprocessorOptions	An object containing option-value pairs to pass to style preprocessors. See more in Styles and scripts configuration below.
          scripts	An object containing JavaScript script files to add to the global context of the project. The scripts are loaded exactly as if you had added them in a <script> tag inside index.html. See more in Styles and scripts configuration below.
          budgets	Default size-budget type and threshholds for all or parts of your app. You can configure the builder to report a warning or an error when the output reaches or exceeds a threshold size. See Configure size budgets. (Not available in test section.)
          fileReplacements	An object containing files and their compile-time replacements. See more in Configure target-specific file replacements.

        Complex configuration values

          The options assets, styles, and scripts can have either simple path string values, or object values with specific fields. The sourceMap and optimization options can be set to a simple Boolean value with a command flag, but can also be given a complex value using the configuration file. 

          Assets configuration
          Styles and scripts configuration
            Style preprocessor options
          Optimization configuration
          Source map configuration

    Workspace npm dependencies

      The Angular Framework, Angular CLI, and components used by Angular applications are packaged as npm packages and distributed via the npm registry

      You can download and install these npm packages by using the npm CLI client, which is installed with and runs as a Node.js® application. By default, the Angular CLI uses the npm client.

      Alternatively, you can use the yarn client for downloading and installing npm packages.

      package.JSON
        
        Both npm and yarn install the packages that are identified in a  package.json file.
    
        The package.json is organized into two groups of packages:

          Dependencies are essential to running applications.
          DevDependencies are only necessary to develop applications.

        Dependencies

          The dependencies section of package.json contains:

            Angular packages: Angular core and optional modules; their package names begin @angular/.

            Support packages: 3rd party libraries that must be present for Angular apps to run.

            Polyfill packages: Polyfills plug gaps in a browser's JavaScript implementation.
              Many browsers lack native support for some features in the latest HTML standards, features that Angular requires. 
              Polyfills can emulate the missing features. The Browser Support guide explains which browsers need polyfills and how you can add them.

          To add a new dependency, use the ng add command.

    TypeScript configuration
      
      At the root tsconfig.json file specifies the base TypeScript and Angular compiler options that all projects in the workspace inherit.



  Application project files

    app/	Contains the component files in which your application logic and data are defined. See details below.
    assets/	Contains image and other asset files to be copied as-is when you build your application.
    environments/	Contains build configuration options for particular target environments. By default there is an unnamed standard development environment and a production ("prod") environment. You can define additional target environment configurations.
    favicon.ico	An icon to use for this application in the bookmark bar.
    index.html	The main HTML page that is served when someone visits your site. The CLI automatically adds all JavaScript and CSS files when building your app, so you typically don't need to add any <script> or<link> tags here manually.
    main.ts	The main entry point for your application. Compiles the application with the JIT compiler and bootstraps the application's root module (AppModule) to run in the browser. You can also use the AOT compiler without changing any code by appending the --aot flag to the CLI build and serve commands.
    polyfills.ts	Provides polyfill scripts for browser support.
    styles.sass	Lists CSS files that supply styles for a project. The extension reflects the style preprocessor you have configured for the project.
    test.ts	The main entry point for your unit tests, with some Angular-specific configuration. You don't typically need to edit this file.

    Inside the src/ folder, the app/ folder contains your project's logic and data. Angular components, templates, and styles go here.

    app/app.component.ts	Defines the logic for the app's root component, named AppComponent. The view associated with this root component becomes the root of the view hierarchy as you add components and services to your application.
    app/app.component.html	Defines the HTML template associated with the root AppComponent.
    app/app.component.css	Defines the base CSS stylesheet for the root AppComponent.
    app/app.component.spec.ts	Defines a unit test for the root AppComponent.
    app/app.module.ts	Defines the root module, named AppModule, that tells Angular how to assemble the application. Initially declares only the AppComponent. As you add more components to the app, they must be declared here.

  Application configuration files

    .browserslistrc	Configures sharing of target browsers and Node.js versions among various front-end tools. See Browserslist on GitHub for more information.
    karma.conf.js	Application-specific Karma configuration.
    tsconfig.app.json	Application-specific TypeScript configuration, including TypeScript and Angular template compiler options. See TypeScript Configuration and Angular Compiler Options.
    tsconfig.spec.json	TypeScript configuration for the application tests. See TypeScript Configuration.
    tslint.json	Application-specific TSLint configuration.

    tsconfig.json 

    When the noImplicitAny flag is false (the default), and if the compiler cannot infer the variable type based on how it's used, the compiler silently defaults the type to any. That's what is meant by implicit any.

    TypeScript typings

      Many JavaScript libraries, such as jQuery, the Jasmine testing library, and Angular, extend the JavaScript environment with features and syntax that the TypeScript compiler doesn't recognize natively. When the compiler doesn't recognize something, it throws an error.

      Use TypeScript type definition files—d.ts files—to tell the compiler about the libraries you lo

    lib.d.ts

      TypeScript includes a special declaration file called lib.d.ts. This file contains the ambient declarations for various common JavaScript constructs present in JavaScript runtimes and the DOM.

    Installable typings files

      Many libraries—jQuery, Jasmine, and Lodash among them—do not include d.ts files in their npm packages. Fortunately, either their authors or community contributors have created separate d.ts files for these libraries and published them in well-known locations.

    target

      By default, the target is es2015, which is supported only in modern browsers. You can configure the target to es5 to specifically support legacy browsers. Differential loading is also provided by the Angular CLI to support modern, and legacy browsers with separate bundles.

    Browser support
      
      Angular supports most recent browsers. This includes the following specific versions:

        Chrome	latest
        Firefox	latest and extended support release (ESR)
        Edge	2 most recent major versions
        IE	
        11
        Safari	2 most recent major versions
        iOS	2 most recent major versions
        Android	Q (10.0), Pie (9.0), Oreo (8.0), Nougat (7.0)

      Angular's continuous integration process runs unit tests of the framework on all of these browsers for every pull request, using Sauce Labs and BrowserStack

      Polyfills

        Angular is built on the latest standards of the web platform. Targeting such a wide range of browsers is challenging because they do not support all features of modern browsers. You compensate by loading polyfill scripts ("polyfills") for the browsers that you must support. 

        In Angular CLI version 8 and higher, applications are built using differential loading, a strategy where the CLI builds two separate bundles as part of your deployed application.

          The first bundle contains modern ES2015 syntax, takes advantage of built-in support in modern browsers, ships less polyfills, and results in a smaller bundle size.

          The second bundle contains code in the old ES5 syntax, along with all necessary polyfills. This results in a larger bundle size, but supports older browsers.

      Enabling polyfills with CLI projects
        
        The Angular CLI provides support for polyfills. If you are not using the CLI to create your projects

        If the polyfill you want is not already in polyfills.ts file, add the import statement by hand.

        ES2015 is mandatory polyfiils - required to run an angular application on each supported browser

      Polyfills for non-CLI users

        If you are not using the CLI, add your polyfill scripts directly to the host web page (index.html).
    
    Strict mode

      When you create a new workspace or an application you have an option to create them in a strict mode using the --strict flag.

      Enabling this flag initializes your new workspace or application with a few new settings that improve maintainability, help you catch bugs ahead of time. Additionally, applications that use these stricter settings are easier to statically analyze, which can help the ng update command refactor code more safely and precisely when you are updating to future versions of Angular.

      Specifically, the strict flag does the following:

      Enables strict mode in TypeScript, as well as other strictness flags recommended by the TypeScript team. Specifically, forceConsistentCasingInFileNames, noImplicitReturns, noFallthroughCasesInSwitch.
      Turns on strict Angular compiler flags strictTemplates, strictInjectionParameters and strictInputAccessModifiers.
      Bundle size budgets have been reduced by ~75%.

      To create a new workspace and application using the strict mode, run the following command:

      content_copy
      ng new [project-name] --strict
      To create a new application in the strict mode within an existing non-strict workspace, run the following command:

      content_copy
      ng generate application [project-name] --Strict

  End-to-end test files

    An e2e/ folder at the top level contains source files for a set of end-to-end tests that correspond to the root-level application, along with test-specific configuration files.

    e2e/
      src/                 (end-to-end tests for my-app)
          app.e2e-spec.ts
          app.po.ts
        protractor.conf.js  (test-tool config)
        tsconfig.json       (TypeScript config inherits from workspace)
        

  Multiple projects
  
    A multi-project workspace is suitable for an enterprise that uses a single repository and global configuration for all Angular projects (the "monorepo" model). A multi-project workspace also supports library development.

    Setting up for a multi-project workspace

      ng new my-workspace --create-application false

      cd my-workspace
      ng generate application my-first-app

  Library project files

    When you generate a library using the CLI (with a command such as ng generate library my-lib), the generated files go into the projects/ folder of the workspace. 

    Libraries (unlike applications and their associated e2e projects) have their own package.json configuration files.

    Under the projects/ folder, the my-lib folder contains your library code.

      src/lib	Contains your library project's logic and data. Like an application project, a library project can contain components, services, modules, directives, and pipes.
      src/test.ts	The main entry point for your unit tests, with some library-specific configuration. You don't typically need to edit this file.
      src/public-api.ts	Specifies all files that are exported from your library.
      karma.conf.js	Library-specific Karma configuration.
      ng-package.json	Configuration file used by ng-packagr for building your library.
      package.json	Configures npm package dependencies that are required for this library.
      tsconfig.lib.json	Library-specific TypeScript configuration, including TypeScript and Angular template compiler options. See TypeScript Configuration.
      tsconfig.spec.json	TypeScript configuration for the library tests. See TypeScript Configuration.
      tslint.json	Library-specific TSLint configuration.
   

    




