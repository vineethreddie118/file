<div class="form-group col-md-6 col-lg-4 col-xl-3">
                        <label for="effectiveDate">Effective Date<span class="required-star"> *</span></label>
                        <div class="input-group">
                          <input maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
                            [ngClass]="{'red-border': ( effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched|| formsubmitted ))}"
                            placeholder="mm/dd/yyyy" name="effectiveDate" #effectiveDate="ngModel"
                            [(ngModel)]="oCurrentProviderLocationInfo.effectiveDate"
                            #dp1="ngbDatepicker" ngbDatepicker/>
                            <i class="cal-icon far fa-calendar" (click)="dp1.toggle()"></i>
                            <div class="error-tooltip">
                              <ng-container *ngIf="effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched)">Invalid date</ng-container>
                          </div>
                        </div>
                      </div>

 <div class="col-md-6 col-lg-4 col-xl-3">
                        <label for="inputData">Effective Date </label>
                        <input type="date" class="form-control form-control-custom" id="effectiveDate" name="effectiveDate{{npiindex}}"
                          placeholder="NPI # Effective Date"
                          [ngModel]="oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[npiindex].effectiveDate | date: 'yyyy-MM-dd'"
                          (ngModelChange)="oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[npiindex].effectiveDate = $event"
                          #effectiveDate="ngModel" />
                      </div>
