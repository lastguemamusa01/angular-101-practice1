# angular-101-practice1

[Edit on StackBlitz ⚡️](https://stackblitz.com/edit/angular-101-practice1)

Angular is an application design framework and development platform for creating efficient 
and sophisticated single-page apps

need nodejs, npm package manager and angular cli installed

A component-based framework for building scalable web applications

ng build --prod   - to create the applicacion in the dist folder to deployment

key architectural elements

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
        {{ product.name }}
    - Property bindings, to help you set values for properties and attributes of HTML elements and 
      pass values to your application's presentation logic.
        <a [title]="product.name + ' details'">
    - event binding
        You can also declare event listeners to listen for and respond to user actions such 
        as keystrokes, mouse movements, clicks, and touches.
    - Directives - (Structural directives) *ngIf and *ngFor or custom directive
        You can use directives to perform a variety of tasks, such as dynamically modifying the DOM structure
    - Dependency injection 
        allows you to declare the dependencies of your TypeScript classes without taking care of their instantiation. 
        Instead, Angular handles the instantiation for you. This design pattern allows you to write more 
        testable and flexible code.
        - example - using componenet declaring in the constructor the service class
    - Angular CLI -is the fastest, easiest, and recommended way to develop Angular applications

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
    
    ng new my-app
    ng generate component <component-name>   

View encapsulation

    component CSS styles are encapsulated into the component's view and don't affect the rest of the application.
