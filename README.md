 <div class="form-group col-md-6 col-lg-4 col-xl-3">
                        <label for="effectiveDate">Effective Date<span class="required-star"> *</span></label>
                        <div class="input-group">
                          <input maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
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
                      


                      <div class="form-group col-md-6 col-lg-4 col-xl-3">
                        <label for="expirationDate">Expiration Date</label>
                        <div class="input-group">
                          <input type="date" class="form-control form-control-custom" id="expirationDate" name="expirationDate"
                            [ngModel]="oCurrentProviderLocationInfo.expirationDate | date: 'yyyy-MM-dd'"
                            (ngModelChange)="oCurrentProviderLocationInfo.expirationDate = $event"
                            #expirationDate="ngModel" />
                          
                        </div>
                      </div>
