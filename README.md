 <div class="form-group col-md-6 col-lg-4 col-xl-3">
                        <label for="effectiveDate">Effective Date<span class="required-star"> *</span></label>
                        <div class="input-container">
                          <input type="date" class="form-control form-control-custom"
                            [ngClass]="{'red-border': ( effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched|| formsubmitted ))}"
                            id="effectiveDate" name="effectiveDate"
                            [ngModel]="oCurrentProviderLocationInfo.effectiveDate | date: 'yyyy-MM-dd'"
                            (ngModelChange)="oCurrentProviderLocationInfo.effectiveDate= $event"
                            #effectiveDate="ngModel" required />
                          <div class="error-tooltip"
                            *ngIf="effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched|| formsubmitted)">
                            Required
                          </div>
                        </div>
                      </div>
