  <div class="col-md-6 col-lg-4 col-xl-3">
                        <label for="inputData">Effective Date </label>
                        <input type="date" class="form-control form-control-custom" id="effectiveDate" name="effectiveDate"
                            placeholder="NPI # Effective Date"
                            [ngModel]="providerForm.providerAlternateIDSection[0].providerNpi[0].effectiveDate | date: 'yyyy-MM-dd'" 
                            (ngModelChange)="providerForm.providerAlternateIDSection[0].providerNpi[0].effectiveDate = $event">
                    </div>
