<!-- app.component.html -->
<h1>Form Validation</h1>

<div>
  <form [formGroup]="userForm" (ngSubmit)="onSubmit()">
    <div>
      <label for="inputScope">Name: - </label>
      <input type="text" id="inputScope" formControlName="name" />
    </div><br>

    <div formArrayName="inputtypes">
      <div *ngFor="let child of getContactControls(); let i = index" formGroupName="{{i}}">
        <label for="inputScope">Type: - </label>
        <select formControlName="type">
          <option value="">Select Type</option>
          <option value="Primary">Primary</option>
          <option value="Secondary">Secondary</option>
          <option value="Home">Home</option>
          <option value="Office">Office</option>
        </select>
        <br><br>
        <label for="contact1">Contact1:</label>
        <input type="text" id="contact1" formControlName="contact1" />
        <br><br>
        <div *ngIf="showContact2">
          <label for="contact2">Contact2:</label>
          <input type="text" id="contact2" formControlName="contact2" />
        </div>
      </div>
    </div><br>

    <button type="button" (click)="toggleContact2()">Toggle Contact2</button>
    <br><br>

    <button type="submit" [disabled]="userForm.invalid">Submit</button>
  </form>
</div>
-----------------------------------------------------------------------

// app.component.ts
import { Component, OnInit } from '@angular/core';
import { FormGroup, FormControl, Validators, FormArray } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  userForm!: FormGroup;
  showContact2: boolean = false;

  constructor() {
    this.createForm();
  }

  ngOnInit(): void {}

  createForm() {
    this.userForm = new FormGroup({
      name: new FormControl('', [Validators.required]),
      inputtypes: new FormArray([
        new FormGroup({
          type: new FormControl(''),
          contact1: new FormControl(''),
          contact2: new FormControl('')
        })
      ])
    });
  }

  getContactControls() {
    return (this.userForm.get('inputtypes') as FormArray).controls;
  }

  toggleContact2() {
    this.showContact2 = !this.showContact2;
    if (!this.showContact2) {
      // Clear the value of contact2 when hiding the input box
      this.userForm.get('inputtypes.0.contact2')?.setValue('');
    }
  }

  onSubmit() {
    const formValues = this.userForm.value;

    const contacts = [];
    
    if (formValues.inputtypes[0].contact1) {
      contacts.push({
        type: formValues.inputtypes[0].type,
        contact1: formValues.inputtypes[0].contact1
      });
    }

    if (this.showContact2 && formValues.inputtypes[0].contact2) {
      contacts.push({
        type: formValues.inputtypes[0].type,
        contact2: formValues.inputtypes[0].contact2
      });
    }

    console.log({
      name: formValues.name,
      contacts: contacts
    });
  }
}
--------------------------------------------------------------------

app.component.html
<h1>Form Validation</h1>

<div>
  <form [formGroup]="userForm" (ngSubmit)="onSubmit()">
    <!-- Input for inputScope -->
    <div>
      <label for="inputScope">Name: - </label>
      <input type="text" id="inputScope" formControlName="name" />
    </div><br>

    <!-- Input for input -->
    <div formArrayName="contact">
      <div *ngFor="let child of getContactControls(); let i = index" formGroupName="{{i}}">
        <label for="inputScope">Type: - </label>
        <select formControlName="type">
          <option value="">Select Type</option>
          <option value="Primary">Primary</option>
          <option value="Secondary">Secondary</option>
          <option value="Home">Home</option>
          <option value="Office">Office</option>
        </select>
        <br><br>
        <label for="contact">Contact:</label>
        <input type="text" id="contact" formControlName="contact" />
      </div>
    </div><br>

    <!-- Submit button -->
    <button type="submit" [disabled]="userForm.invalid">Submit</button>
  </form>
</div>
-------------------------------------------------------

// // app.component.ts
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, FormArray, Validators } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  userForm!: FormGroup;

  constructor(private fb: FormBuilder) {
    this.createForm();
  }

  ngOnInit(): void {}

  createForm() {
    this.userForm = this.fb.group({
      name: ['', Validators.required],
      contact: this.fb.array([
        this.fb.group({
          type: [''],
          contact: ['']
        })
      ])
    });
  }

  getContactControls() {
    return (this.userForm.get('contact') as FormArray).controls;
  }

  onSubmit() {
    // Access the form values
    const formValues = this.userForm.value;

    // Log the form values to the console
    console.log(formValues);

    // Perform any other actions you need with the form values
  }
}
// app.component.ts
// import { Component, OnInit } from '@angular/core';
// import { FormGroup, FormControl, Validators, FormArray } from '@angular/forms';

// @Component({
//   selector: 'app-root',
//   templateUrl: './app.component.html',
//   styleUrls: ['./app.component.css']
// })
// export class AppComponent implements OnInit {
//   userForm!: FormGroup;

//   constructor() {
//     this.createForm();
//   }

//   ngOnInit(): void {}

//   createForm() {
//     this.userForm = new FormGroup({
//       name: new FormControl('', [Validators.required]),
//       contact: new FormArray([
//         new FormGroup({
//           type: new FormControl(''),
//           contact: new FormControl('')
//         })
//       ])
//     });
//   }

//   getContactControls() {
//     return (this.userForm.get('contact') as FormArray).controls;
//   }

//   onSubmit() {
//     // Access the form values
//     const formValues = this.userForm.value;

//     // Log the form values to the console
//     console.log(formValues);

//     // Perform any other actions you need with the form values
//   }
// }
