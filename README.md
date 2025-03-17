 <div class="form-group col-md-6 col-lg-4 col-xl-3">
                      <label for="expirationDate">Expiration Date</label>
                      <div class="input-container">
                        <input type="date" class="form-control form-control-custom" id="expirationDate" name="expirationDate"
                          [ngModel]="oCurrentProviderLocationInfo.expirationDate | date: 'yyyy-MM-dd'"
                          (ngModelChange)="oCurrentProviderLocationInfo.expirationDate = $event"
                          #expirationDate="ngModel" />
                        
                      </div>
                    </div>
