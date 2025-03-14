<div class="col-md-6 col-lg-4 col-xl-3">
                        <label for="inputData">Expiration Date</label>
                        <div class="input-group">
                            <input maxlength="10" dateFormater class="data-input form-control"
                                [ngClass]="{'red-border': (dc3.invalid && (dc3.dirty || dc3.touched ))}"
                                placeholder="mm/dd/yyyy" name="expirationDate" #dc3="ngModel"
                                [(ngModel)]="providerForm.providerAlternateIDSection[0].providerNpi[0].expirationDate" 
                                #dp3="ngbDatepicker" ngbDatepicker/>
                            <i class="cal-icon far fa-calendar" (click)="dp3.toggle()"></i>
                            <div class="error-tooltip">
                                <ng-container *ngIf="dc3.invalid && (dc3.dirty || dc3.touched)">Invalid date</ng-container>
                            </div>
                        </div>
                    </div>
