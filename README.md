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

    - Templates
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
    
