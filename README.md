 <div class=" col-md-6 col-lg-4 col-xl-3">
                        <label for="inputData">Effective Date<span class="required-star"> *</span></label>
                        <div class="input-group">
                          <input maxlength="10" dateFormater class="data-input form-control"
                            [ngClass]="{'red-border': ( dc1.invalid && (dc1.dirty || dc1.touched|| formsubmitted ))}"
                            placeholder="mm/dd/yyyy" name="effectiveDate" #dc1="ngModel"
                            [(ngModel)]="oCurrentProviderLocationInfo.effectiveDate"
                            #dp1="ngbDatepicker" ngbDatepicker/>
                            <i class="cal-icon far fa-calendar" (click)="dp1.toggle()"></i>
                            <div class="error-tooltip">
                              <ng-container *ngIf="dc1.invalid && (dc1.dirty || dc1.touched)">Invalid date</ng-container>
                          </div>
                        </div>
                      </div>


<div class="col-md-6 col-lg-4 col-xl-3">
                        <label for="inputData">Effective Date</label>
                        <div class="input-group">
                            <input maxlength="10" dateFormater class="data-input form-control"
                                [ngClass]="{'red-border': (dc2.invalid && (dc2.dirty || dc2.touched ))}"
                                placeholder="mm/dd/yyyy" name="effectiveDate" #dc2="ngModel"
                                [(ngModel)]="providerForm.providerAlternateIDSection[0].providerNpi[0].effectiveDate" 
                                #dp2="ngbDatepicker" ngbDatepicker/>
                            <i class="cal-icon far fa-calendar" (click)="dp2.toggle()"></i>
                            <div class="error-tooltip">
                                <ng-container *ngIf="dc2.invalid && (dc2.dirty || dc2.touched)">Invalid date</ng-container>
                            </div>
                        </div>
                    </div>
