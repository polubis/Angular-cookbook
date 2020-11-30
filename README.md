# Angular-cookbook

## Forms - ngModel

- **FormsModule**
- Using `formRef` allows read informations like **invalid**, **dirty**, **value**, ...etc.
- [ngModel] creates new value and do not change original
- [(ngModel)] binds with original value and changes
- (ngSubmit) - prevents default,
- (ngModelChange) triggers event after value change

```html
 <form #formRef (ngSubmit)="onClickBeforeSaveIndicationData()">
<!--                name="formHandler" novalidate-->
                <fieldset [disabled]="workInProgress">

                    <table class="admin-table">
                        <thead>
                            <tr>
                                <th></th>
                                <th class="name-input">Steps in the patient funnel</th>
                                <th class="align-center">Default value</th>
                                <th class="align-center">Estimate</th>
                                <th class="align-center"></th>
                            </tr>
                        </thead>

                        <tbody>
                            <tr *ngIf="labels.length == 0">
                                <td [attr.colspan]="100" style="text-align:center;">No rows available</td>
                            </tr>

                            <tr *ngFor="let labelRecord of labels; let i = index" [ngClass]="{'not-sortable': labelRecord.locked}">
                                <td class="name-input">
                                    <input
                                        type="text"
                                        class="align-left"
                                        minlength="1"
                                        maxlength="100"
                                        required
                                        [(ngModel)]="labelRecord.name"
                                        [name]="labelRecord.name"
                                    />
<!--                                    ng-model="labelRecord.name"-->
<!--                                    ng-minlength="1"-->
<!--                                    ng-maxlength="100"-->
<!--                                    required-->
                                </td>
                                <td>
                                    <div *ngIf="labelRecord.order == 1">
                                        <input
                                            type="number"
                                            class="align-center"
                                            min="0"
                                            max="100000000000"
                                            required
                                            [(ngModel)]="labelRecord.value"
                                            (ngModelChange)="recalculateValues()"
                                            [name]="labelRecord.value"
                                        />

<!--                                        ng-model="labelRecord.value"-->
<!--                                        ng-change="$ctrl.recalculateValues()"-->
<!--                                        min="0"-->
<!--                                        max="100000000000"-->
<!--                                        ng-pattern="/^[0-9]+$/"-->
<!--                                        required-->
                                    </div>
                                    <div *ngIf="labelRecord.order != 1">
                                        <div class="label-percent">
                                            <input
                                                type="number"
                                                class="align-center"
                                                min="0"
                                                max="100"
                                                step="any"
                                                required
                                                [(ngModel)]="labelRecord.value"
                                                (ngModelChange)="recalculateValues()"
                                                [name]="labelRecord.value"
                                            />
<!--                                            ng-model="labelRecord.value"-->
<!--                                            ng-change="$ctrl.recalculateValues()"-->
<!--                                            min="0"-->
<!--                                            max="100"-->
<!--                                            step="any"-->
<!--                                            ng-pattern="/^[0-9]+(\.[0-9]{1,5})?$/"-->
<!--                                            required-->
                                            <span>%</span>
                                        </div>
                                    </div>
                                </td>
                            </tr>
                        </tbody>
                    </table>

                    <!-- Form buttons -->
                    <div class="text-center" *ngIf="!workInProgress" style="margin-top: 20px;">
                        <button type="button"
                                class="mark-btn mark-btn-outline-secondary"
                                (click)="returnToIndicationList()"
                                [disabled]="workInProgress">
                            Cancel
                        </button>
                        <button type="submit"
                                class="mark-btn mark-btn-outline-primary"
                                [disabled]="workInProgress || (formRef.dirty && formRef.invalid)">
                            <!--                                || $ctrl.formHandler.$pristine || $ctrl.formHandler.$invalid-->
                            Update
                        </button>
                    </div>

                </fieldset>

            </form>
```
