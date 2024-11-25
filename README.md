import { Injectable } from '@angular/core';

@Injectable({
providedIn: 'root', // Ensures it's a singleton
})
export class FormDataService {
private formData: any = {}; // Object to hold the form data for all steps

// Save data for a specific step
setFormData(step: number, data: any): void {
this.formData[step] = data;
}

// Retrieve data for a specific step
getFormData(step: number): any {
return this.formData[step] || {}; // Return empty object if no data
}

// Retrieve all form data (optional, for submission)
getAllFormData(): any {
return this.formData;
}
}

service code

component level code

import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup } from '@angular/forms';
import { FormDataService } from '../form-data.service';

@Component({
selector: 'app-step1',
template: `   <form [formGroup]="form" (ngSubmit)="onContinue()">
      <label>First Name:</label>
      <input formControlName="firstName" />
      <label>Last Name:</label>
      <input formControlName="lastName" />
      <button type="submit">Continue</button>
    </form>
`,
})
export class Step1Component implements OnInit {
form!: FormGroup;

constructor(private fb: FormBuilder, private formDataService: FormDataService) {}

ngOnInit() {
this.form = this.fb.group({
firstName: [''],
lastName: [''],
});

    // Load saved data into the form
    const savedData = this.formDataService.getFormData(1);
    this.form.patchValue(savedData);

}

onContinue() {
// Save the form data
this.formDataService.setFormData(1, this.form.value);

    // Navigate to the next step
    // (Use your router logic here)

}
}
