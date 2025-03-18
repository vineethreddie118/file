 <div class="form-row" *ngIf="isReplaceMode">
                  <div class="form-group col-12" style="display:inline-block">
                    <label for="locationExpirationDate">Expiration Date<span class="required-star"> *</span></label>
                    <input style="max-width:10rem" type="date" class="form-control form-control-custom" id="locationExpirationDate"
                      name="locationExpirationDate"
                      [ngClass]="{'red-border': repExpDate.invalid && (repExpDate.dirty || repExpDate.touched ||formsubmitted)}"
                      [(ngModel)]="replaceLocationExpirationDate" #repExpDate="ngModel" required />
                    <div style="color:red"
                      *ngIf="repExpDate.invalid && (repExpDate.dirty || repExpDate.touched ||formsubmitted)">
                      Expiration Date is Required to replace a location
                    </div>
                  </div>
                </div>
