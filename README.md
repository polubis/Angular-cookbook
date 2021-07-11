# Angular-cookbook

## Reactive forms

```ts
form = new FormGroup({
	name: new FormControl('', [Validators.required, Validators.minLength(3), Validators.minLength(100)]),
});

// form [formGroup]="form"
// formControlName="name"
```

## Template driven forms

- **FormsModule**
- Using `formRef` allows read informations like **invalid**, **dirty**, **value**, ...etc.
- [ngModel] creates new value and do not change original
- [(ngModel)] binds with original value and changes
- (ngSubmit) - prevents default,
- (ngModelChange) triggers event after value change

```html
<form name="form" (ngSubmit)="f.form.valid && onSubmit()" #f="ngForm" novalidate>
<div class="form-group">
<label for="firstName">First Name</label>
<input type="text" class="form-control" name="firstName" [(ngModel)]="model.firstName" #firstName="ngModel" [ngClass]="{ 'is-invalid': f.submitted && firstName.invalid }" required pattern="[0-9]" />
<div *ngIf="f.submitted && firstName.invalid" class="invalid-feedback">
	<div *ngIf="firstName.errors.required">First Name is required</div>
	<div *ngIf="firstName.errors.pattern">Invalid format</div>
</div>
</div>
<div class="form-group">
<label for="lastName">Last Name</label>
<input type="text" class="form-control" name="lastName" [(ngModel)]="model.lastName" #lastName="ngModel" [ngClass]="{ 'is-invalid': f.submitted && lastName.invalid }" required />
<div *ngIf="f.submitted && lastName.invalid" class="invalid-feedback">
	<div *ngIf="lastName.errors.required">Last Name is required</div>
</div>
</div>
<div class="form-group">
<label for="email">Email</label>
<input type="text" class="form-control" name="email" [(ngModel)]="model.email" #email="ngModel" [ngClass]="{ 'is-invalid': f.submitted && email.invalid }" required email />
<div *ngIf="f.submitted && email.invalid" class="invalid-feedback">
	<div *ngIf="email.errors.required">Email is required</div>
	<div *ngIf="email.errors.email">Email must be a valid email address</div>
</div>
</div>
<div class="form-group">
<label for="password">Password</label>
<input type="password" class="form-control" name="password" [(ngModel)]="model.password" #password="ngModel" [ngClass]="{ 'is-invalid': f.submitted && password.invalid }" required minlength="6" />
<div *ngIf="f.submitted && password.invalid" class="invalid-feedback">
	<div *ngIf="password.errors.required">Password is required</div>
	<div *ngIf="password.errors.minlength">Password must be at least 6 characters</div>
</div>
</div>
<div class="form-group">
<button class="btn btn-primary">Register</button>
</div>
</form>
```

### Creating own validator directive

```ts
import { Directive, Input } from '@angular/core';
import { NG_VALIDATORS, Validator, AbstractControl, Validators } from '@angular/forms';

@Directive({
    selector: '[min]',
    providers: [{ provide: NG_VALIDATORS, useExisting: MinValidatorDirective, multi: true }],
})
export class MinValidatorDirective implements Validator {
    @Input() min: number;

    validate(control: AbstractControl): { [key: string]: any } {
        return Validators.min(this.min)(control);
    }
}
```


## Material UI utils

### Creating Confirm Dialog

```ts
  constructor(
        public matDialog: MatDialog
    ) {}

const dialogMessage =
	res.affectedAdaptations.length > 0
	    ? this._generateMessageBasedOnAffectedAdaptations(
		  res.affectedAdaptations
	      )
	    : 'Update the Top-down forecast?';

    const dialogRef = this.matDialog.open(ConfirmationDialogComponent, {
	data: {
	    title: 'Are you sure?',
	    content: dialogMessage,
	    ok: 'Yes',
	    cancel: 'No',
	},
    });

    dialogRef.afterClosed().subscribe((result) => {
	console.log(result);
	if (result) {
	    this._saveIndicationData();
	}

	this.workInProgress = false;
    });
```
```ts
import { Component, Inject } from '@angular/core';
import { downgradeComponent } from '@angular/upgrade/static';
import { MAT_DIALOG_DATA } from '@angular/material/dialog';

@Component({
    selector: 'app-confirmation-dialog',
    template: `
        <h2 mat-dialog-title>{{ data.title }}</h2>
        <mat-dialog-content class="mat-typography">
            {{ data.content }}
            Update the Top-down forecast
        </mat-dialog-content>
        <mat-dialog-actions align="end">
            <button mat-button mat-dialog-close>{{ data.cancel }}</button>
            <button mat-button [mat-dialog-close]="true" cdkFocusInitial>{{ data.confirm }}</button>
        </mat-dialog-actions>
    `,
})
export class ConfirmationDialogComponent {
    constructor(@Inject(MAT_DIALOG_DATA) public data: any) {}
}
```
