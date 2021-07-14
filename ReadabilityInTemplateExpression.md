Readability

The following code is for loading a grid (a simple html table) with data from api. We have a print button to print those data. loading boolean property of the component 
is used to disable print button when data is geting from api and processing to populate in the grid.

in component.html

```angular
<button label="print" [disabled]="loading"></button>
```

in component.ts
```typescript
public loading: boolean = false;
loadEmployeeData() {
   this.loading = true;  //setting boolean flag to indicate loading process is in progress
   this.service.getEmployees().subscribe((response) => {
      .....
      //assign data from response to show it as grid
      ....
      this.loading = false;  //at last seting boolean flag to indicate loading is completed
   });
}
```

Above code is working fine. Now we got a change Requirement/issue so that we need to disable print button when grid has zero records . How to achive that ? The response object 
contains data property. response.data is an array containing employee data

My first solution
in component.html
```html
<button label="print" [disabled]="!IsPrintEnabled || loading"></button> //using truth table i come up with expression
                                                                        // alternatively "IsPrintEnabled ? loading : !IsPrintEnabled" will be more readable i guess
```                                                                        

in component.ts

```typescript
public loading: boolean = false;
public IsPrintEnabled: boolean = false;
loadEmployeeData() {
   this.loading = true;  //setting boolean flag to indicate loading process is in progress
   this.service.getEmployees().subscribe((response) => {
      .....
      //assign data from response to show it as grid
      ....
      this.IsPrintEnabled = response.data.length > 0;
      this.loading = false;  //at last seting boolean flag to indicate loading is completed
   });
}
```


What if change IsPrintEnabled to IsEmpty

in component.html

```html
<button label="print" [disabled]="IsEmpty || loading"></button> 
```

in component.ts
```typescript
public loading: boolean = false;
public IsEmpty: boolean = false;
loadEmployeeData() {
   this.loading = true;  //setting boolean flag to indicate loading process is in progress
   this.service.getEmployees().subscribe((response) => {
      .....
      //assign data from response to show it as grid
      ....
      this.IsEmpty = response.data.length === 0;
      this.loading = false;  //at last seting boolean flag to indicate loading is completed
   });
}
```

`<button [disabled]="IsEmpty">Print</button>` is better than  `<button [disabled]="!IsExportEnabled">Print</button>`
