  <div class="col-md-6 col-lg-4 col-xl-3 position-relative">
    <label for="effectiveDate">Effective Date</label>
    <div class="input-group">
        <input type="text" class="form-control form-control-custom" id="effectiveDate" name="effectiveDate"
            placeholder="MM/DD/YYYY"
            [ngModel]="providerForm.providerAlternateIDSection[0].providerNpi[0].effectiveDate | date: 'MM/dd/yyyy'" 
            (ngModelChange)="providerForm.providerAlternateIDSection[0].providerNpi[0].effectiveDate = $event">
        <div class="input-group-append">
            <span class="input-group-text cursor-pointer" (click)="effectiveDatePicker.click()">
                <i class="fa fa-calendar"></i>
            </span>
        </div>
    </div>
    <input type="date" class="d-none" #effectiveDatePicker 
        (change)="providerForm.providerAlternateIDSection[0].providerNpi[0].effectiveDate = $event.target.value">
</div>
