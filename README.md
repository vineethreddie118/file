 <div class="col-md-6 col-lg-4 col-xl-3">
                        <label for="inputData">Expiration Date </label>
                        <input type="date" class="form-control form-control-custom" id="expirationDate" name="expirationDate{{npiindex}}"
                          placeholder="NPI # Expiration Date"
                          [ngModel]="oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[npiindex].expirationDate | date: 'yyyy-MM-dd'"
                          (ngModelChange)="oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[npiindex].expirationDate = $event"
                          #expirationDate="ngModel" />
                      </div>
