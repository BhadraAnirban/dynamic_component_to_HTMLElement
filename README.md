# Add an angular component dynamically to a HTML Element
## We will pass data to the component properties.
## It will also work if you are using [innerHTML] to render the html content coming from Server.

```
<div [innerHtml]="templateString"></div>
```

### Create a Component (here I have created - SignatureControlComponent) and add it to the entryComponent of module.


```
@Component({
  selector: 'app-signature-control',
  templateUrl: './signature-control.component.html',
  styleUrls: ['./signature-control.component.scss']
})
export class SignatureControlComponent implements OnInit {
role: string;
}
```

In the app.nmodule-
```
entryComponents: [SignatureControlComponent]
```
### In the component (I have used EditTemplateComponent) where you are going to create the component and add to the HTML element.

Inject ComponentFactoryResolver, ViewContainerRef, Renderer2 using the constructor.
```
constructor( 
  private elem: ElementRef, private viewContainerRef: ViewContainerRef,
  private componentFactoryResolver: ComponentFactoryResolver, private renderer: Renderer2) { }

```

Use ngAfterViewChecked life cycle hook to get HTML element using class (I have used the classname 'Signature'). This will also work [innerHTML] to render the html content coming from Server.
```
ngAfterViewChecked() {
    if(!this.isElementsRendered)
    {
      
      let elements = this.elem.nativeElement.querySelectorAll('.Signature');
      if(elements.length > 0)
      {
        this.isElementsRendered = true;
        elements.forEach(element => {
          this.ModifySignature(element);
        })
      }
    }   
  }
```
Finally create the instance of the component and append to the element.
```
ModifySignature(element: HTMLElement){    
    const componentFactory = this.componentFactoryResolver.resolveComponentFactory(SignatureControlComponent);    
    let componentInstance = this.viewContainerRef.createComponent(componentFactory);
    const loaderComponentElement = componentInstance.location.nativeElement;
  // Assign value to the property
    componentInstance.instance.role = 'PMKJ!';

    this.renderer.appendChild(element, loaderComponentElement);
  }

```
