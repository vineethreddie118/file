 <div class="form-row">
                <div class="form-group col-12" style="display:inline-block">
                  <label for="billingExpirationDate">Expiration Date for old location<span class="required-star">
                      *</span></label>
                  <input style="max-width:10rem" type="date" class="form-control form-control-custom" #expDate="ngModel"
                    id="billingExpirationDate" name="billingExpirationDate"
                    [ngClass]="{'red-border':expDate.invalid && (expDate.dirty || expDate.touched ||formsubmitted)}"
                    [(ngModel)]="replaceBillingExpirationDate" required />
                 
                  <div style="color:red" *ngIf="expDate.invalid && (expDate.dirty || expDate.touched ||formsubmitted)">
                    *Expiration Date is Required to replace a billing location
                  </div>
                </div>
              </div>
