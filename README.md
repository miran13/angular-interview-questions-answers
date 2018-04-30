# angular-interview-questions-answers
Angular  Interview Questions and Answers


# What is an Angular?

Angular is a platform and framework to build client application in HTML and TypeScript.
The basic building blocks of an angular application are NG- Module, which provide a  compilation context for components.
Below are the core concepts
	1) Module
	2) Components
	3) Service
	4) Route
	5) Template
	6) Forms 
	   Template driven form
	   Reactive form
	7) HTTPClient
	8) Dependency Injection
	9) Providers
	10) Injectors
	
# What is a module in Angular ?
Every application has a root module conventionally named as appModule. 
which provides bootstrap mechanism that launches the application.
Typically It contains many functional module.
Ngmodule is a container.
An ngmodule defined as class decorated with @NgModule.  

Below are the import properties of NgModule.
declarions- componets, directives and pipe are belong to this
export- The subset of decalation that should be visiable and usable in 
import -othere modules which are exported 
providers :  where services are be defined.
bootstrap- The main application view , called the root components .

Example

import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
@NgModule({
  imports:      [ BrowserModule ],
  providers:    [ Logger ],
  declarations: [ AppComponent ],
  exports:      [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }



# What is the component in Angular ?
Component is a class. Component decorator allows to mark as an Angular component .
Components are the most basic building block of an UI in an Angular application. 
It provides additional metadata that determines how the component should be processed, instantiated and used at runtime.
A component needs to configured in NgModule in order to be usable by another component or application.

 Example -
@Component({
  selector:    'app-customer-list',
  templateUrl: './customer-list.component.html',
  providers:  [ CustomerService ]
})
export class CustomerListComponent implements OnInit {
/* . . . */
}

# What is the dependency Injection ?
Dependency Injection is often called DI. Dependency Injection is wired into an application with the help of providers and used in a component .

# What is the providers ?
The providers are used to configure the service in component to NgModule class. Andinjector can use the create a new instance.

# How parent component pass the data to child component ?
	1) Pass the data from parent to child with input binding
	2) Intercept input property changes with a setter
	3) Intercept input property changes with the ngOnChanges
	4) Parent listens to child event
	5) Parent interacts with child via local variable
	6) Parent calls on @Vaildchild
	7) Parent and children communicate via a servi

# What are the phases in an angular life cycle?
	1) ngOnChanges()
	2) ngOnInit
	3) ngDoCheck()
	4) ngAfterContentInit()
	5) ngAfterContentChecked
	6) ngAfterViewInit()
	7) ngAfterViewChecked()
	8) ngDestory()

# What is the service?
The service is a class. An injector decorator allows to mark as an angular service.
Service serves data to components.

# What is ngZone?
The ngZone is a service. It executes task inside or outside of angular.
For example. Sometime UI doesn't want to execution, but excepting directly result. 

# What is Polyfills ?
The ployfills supports to all modern browsers.

# How many types of form?

  # Reactive forms are based on reactive style of programming. Following are key aspects of reactive forms:
	• These forms are synchronous in nature unlike template-driven forms.
	• These forms require inclusion/import of ReactiveFormsModule in the root module
	• Form controls are created and managed in component code rather than the UI unlike the template-driven forms.
	• Validation logic is placed within Component rather than the UI code as like template driven 
 # Template Driven Form
	• Template-driven forms is create like native way.
	• These forms are asynchronous in nature.
	• These forms require import of FormsModule in the root module
	• In the form, ngModel is used for two-way data binding
	• Validation related login is placed into the UI code.

# What are the properties of Activated route?
	•  snapshot
	•   url
	•   params
	•   queryParams
	•   fragment
	•   data
	•   outlet

# What are the router events ?
	•  NavigationStart
	• NativationEnd
	• NavigationCancel
	• NavigationError
	• RoutesRecongized
	• RouteConfigLoadStart
	• RouteConfigLoadEnd

# What is Injector bubbling?
When a component request to dependency. Angular tries to satisfy that dependency with a provider registered in that component of own injector. If component's injectors not able to resolve the dependency,It passes the request up to its parent component's injector. If that injector can't satisfy the request , It passes to root component
until it's satisfy. If It runs out of ancestors, angular throws an error.
