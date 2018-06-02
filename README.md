# Angular  Interview Questions and Answers

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
	
# What is a module?
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



# What is a component?
The component is a class. Component decorator allows to mark as an Angular component .
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
	# 1) Pass the data from parent to child with input binding
	
	Example. ParentComponent.ts
	import {
	  Component,
	  OnInit
	} from '@angular/core';

	@Component({
	  selector: 'app-parent',
	  template: `
	    <p>
	      parent works!
	      <app-child [childMessage]="parentMessage"></app-child>
	    </p>
	  `
	})
	export class ParentComponent {
	  parentMessage = 'A Message passed from parent to child';
	}

         ChildComponent.ts
	 import {
 	 Component,
 	 Input
	} from '@angular/core';

	@Component({
 	 selector: 'app-child',
 	 template: `
  	  <p>
  	    child works  =>  {{childMessage}}
   	 </p>  `
	})
	export class ChildComponent {
 	 @Input() childMessage;
	}

	
	# 2) Intercept input property changes with a setter
	
	Example. ParentComponent.ts
	import {
	  Component,
	  OnInit
	} from '@angular/core';

	@Component({
	  selector: 'app-parent',
	  template: `
	    <p>
	      parent works!
	      <app-child [childMessage]="parentMessage"></app-child>
	    </p>
	  `
	})
	export class ParentComponent {
	  parentMessage = 'A Message passed from parent to child';
	}
	ChildComponent.ts
	
	import {
  	Component,
  	Input
	} from '@angular/core';

	@Component({
  	selector: 'app-child',
 	 template: `   <p>  child works  =>  {{childMessage}}  </p>   `
       })
	export class ChildComponent {
  	private _childMessage = '';
  
  	@Input()
  	set childMessage(name: string) {
    	this._childMessage = name
         }

  	get childMessage(): string {
    	return this.childMessage = this._childMessage;
       }
       }

	# 3) Intercept input property changes with the ngOnChanges
	
	Example. 
	Parent.component.ts
	import {
 	 Component,
  	OnInit
	} from '@angular/core';

	@Component({
  	selector: 'app-parent',
  	template: `
    	<p>
      	parent works!
      	<input type="text" [(ngModel)]="parentMessage" >
       	<app-child [childMessage]="parentMessage"></app-child>
    	</p>`})
	
	export class ParentComponent {

  	parentMessage = 'Parent Message';

  	updateParentMessage(parentMsg:string) {
      	this.parentMessage=parentMsg; }
	}
	
	Child.Component.ts
	import {
  	Component,
  	Input,
  	OnChanges,
  	SimpleChanges,
  	SimpleChange
	} from '@angular/core';

	@Component({
  	selector: 'app-child',
  	template: ` <p>   New a message from parent to child   =>  {{ newChildMessage }}   </p>   ` })
       export class ChildComponent implements OnChanges {

       @Input() childMessage: string

       oldChildMessage: string;

       newChildMessage: string;

       ngOnChanges(changes: {  [propKey: string]: SimpleChange }) {

       for (let propName in changes) {
         let changedProp = changes[propName];
         if(!changedProp.isFirstChange() ){ 
         this.newChildMessage = JSON.stringify(changedProp.previousValue);
       }
     }
    }
  }
	
	4) Parent listens to child event
	
		Example.
		Parent.component.ts
		import { Component } from '@angular/core';

		@Component({
		  selector: 'app-parent',
		  template: `<p>
			     Parent message  <input type="text" [(ngModel)]="parentMessage">
			    <app-child (childReceiveMessage)="parentReceiveMessage($event)" [childMessage]="parentMessage"></app-child>
			     </p> `
		}) 
		export class ParentComponent {
		}

		child.component.ts

		import { 
		  Component,
		  Input,
		  EventEmitter,
		  Output
		} from '@angular/core';

		@Component({
		  selector: 'app-child',
		  template: `<p> Child Message <input #childMsg (keyup)="onKey(childMsg.value)" value="{{childMessage}}"> </p>`
		  })

		export class ChildComponent {

		  @Input() childMessage: string;

		  @Output() childReceiveMessage = new EventEmitter<string>();

		  onKey(childMsg: string) {
		    this.childReceiveMessage.emit(childMsg);
		  }

		}

	
	5) Parent interacts with child via local variable
	Example.
	Parent.component.ts
		import { Component } from '@angular/core';

	@Component({
	  selector: 'app-parent',
	  template: `<p>
		    <button (click)="child.callChild()"> Call child from parent </button>
		      <app-child #child></app-child>
		     </p> `
	}) 
	export class ParentComponent {

	  parentMessage = 'Initial Parent Message';

	  parentReceiveMessage(msg:string){
		this.parentMessage=msg;
	      }
	}

      Child.component.ts
	import { Component,Input,EventEmitter,Output } from '@angular/core';

		@Component({
		  selector: 'app-child',
		  template: `
			    <div *ngIf="childMessage">
			    <p>
			    {{childMessage}} => Parent interacts with child via local variables
			    </p>
			    </div>`
		  })

		export class ChildComponent {

		childMessage: string;

		 callChild() {
		  this.childMessage="Child Message";
		  }

		}
			
	6) Parent calls on @Vaildchild
	
	https://stackblitz.com/github/dharmendra1234/Angular_examples-component-interactions
	
	import { Component, ViewChild } from '@angular/core';
	import { ChildComponent } from '../child/child.component';


	@Component({
	  selector: 'app-parent',
	  template: `<p>
		    <button (click)="callChild()"> Call child from parent </button>
		      <app-child #child></app-child>
		     </p> `
	}) 
	export class ParentComponent {
	  @ViewChild(ChildComponent)
	  private childComponent: ChildComponent;

	  callChild(){
	    this.childComponent.callChild();
	  }
	}
	
	import { Component,Input,EventEmitter,Output } from '@angular/core';

	@Component({
	  selector: 'app-child',
	  template: `
		    <div *ngIf="childMessage">
		    <p>
		    {{childMessage}} => Parent calls an @ViewChild()
		    </p>
		    </div>`
	  })

	export class ChildComponent {

	childMessage: string;

	  callChild() {
	  this.childMessage="Child Message";
	  }

	}

	7) Parent and children communicate via a service
	
	

# What are the phases in an angular life cycle?
	1) ngOnChanges()
	2) ngOnInit
	3) ngDoCheck()
	4) ngAfterContentInit()
	5) ngAfterContentChecked
	6) ngAfterViewInit()
	7) ngAfterViewChecked()
	8) ngDestory()

# What is a service?
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
	• NavigationStart
	• NativationEnd
	• NavigationCancel
	• NavigationError
	• RoutesRecongized
	• RouteConfigLoadStart
	• RouteConfigLoadEnd

# What is Injector bubbling?
When a component request to dependency. Angular tries to satisfy that dependency with a provider registered in that component of own injector. If component's injectors not able to resolve the dependency,It passes the request up to its parent component's injector. If that injector can't satisfy the request , It passes to root component
until it's satisfy. If It runs out of ancestors, angular throws an error.

# How Angular 2 is different than angular 1 in term of performance ?

Angular 1 uses two-way data bindings to keep views in sync with data models. This creates a bi-directional MVVM relationship between views and models. If a user input is updated in the view, it will trigger the digest cycle to recursively check the DOM for property changes. Likewise, if the underlying data changes from an asynchronous event (like an $http request), then the same recursive check fires.

While this keeps views in sync with data models, it can quickly cause performance issues if enough watchers are added to the DOM. Since the entire DOM is recursively checked for each event, the digest cycle can cause unpredictable cascading changes that slow things down exponentially.

Unidirectional data flow
Unlike Angular 1's two-way data bindigns, Angular 2 uses unidirectional data flow to detect changes. Similar to React's flux architecture, data flows in one direction every time a change event occurs.

Angular 2 leverages the component tree to ensure that data always flows from top to bottom. Every component you create in Angular 2 has its own change detector. When a user event occurs, change detection occurs from top to bottom in the component tree, starting with the root component.

This top down strategy is more predictable than the digest cycle. It ensures everything gets updated in one run. Additionally, we know that every change we see in our view came from the underlying component (and not from changes made to the view itself).
Why is this better?
Angular 2 change detection works better with modern just-in-time (JIT) compilers. Since every component essentially has it's own blueprint, the browser avoids having to dynamically compare properties at runtime. This drastically improves performance as the compiler can more accurately predict app behavior.

This method of change detection also allows Angular to only update the parts of the DOM that have actually changed. While Angular 1 updates the entire DOM with each check, Angular 2 only redraws the elements that need updating. Since DOM manipulation is one of the most expensive JavaScript operations, this drastically improves speed and efficiency.

Angular 2 Manual Change Detection
You can manually control change detection for a component by explicitly setting the changeDetection property in the @Component decorator. For example, if you set changeDetection: ChangeDetectionStrategy.OnPush then your component will only fire change detection when its inputs change (and not for every event).

Immutability and OnPush
The main advantage of OnPush is gives you more control over change detection. By specifying when a component's change detector fires, you can avoid running unnecessary checks when inputs for that component haven't change.

The one caveat with this is that OnPush only works with immutable objects. You must create a new reference to that property for the component to recognize the change. Since JavaScript objects are inherently mutable, you have to make sure you are creating a new reference to the property or change detection won't fire.
