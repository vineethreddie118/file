<div class="form-group col-md-6 col-lg-4 col-xl-3">
                      <label for="effectiveDate">Effective Date<span class="required-star"> *</span></label>
                      <div class="input-container">
                        <input type="date" class="form-control form-control-custom"
                          [ngClass]="{'red-border': ( effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched ))}"
                          id="effectiveDateupdate" name="effectiveDateupdate"
                          [ngModel]="selectedBillingAddress.effectiveDate | date: 'yyyy-MM-dd'"
                          (ngModelChange)="selectedBillingAddress.effectiveDate= $event" #effectiveDate="ngModel"
                          required />
                        <!-- <div class="error-tooltip error-tooltip-offset"
                          *ngIf="boardEffectiveDateModel.invalid && (boardEffectiveDateModel.dirty || boardEffectiveDateModel.touched || boardCertificationformsubmitted)">
                          Required
                      </div> -->
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
