<div class="form-row border p-2 mb-2">
          <app-multiselect-dropdown 
            #serviceLocationMultiSelect
            class="col-8 p-2" 
            [inputOptions]="oProviderLocation.providerLocationInfo"
            [generateLabelFunction]="getLabel"
            placeholder="Select Service Location"
            (selectedValues)="updateSelectedPracticeLocations($event)"
            customToolTip="true"
            [toolTipTemplate]="toolTipTemplate"
            >
          </app-multiselect-dropdown>

<ng-template #toolTipTemplate let-option="option">
  <div>
    <div>Billing Locations</div>
    <ul class="list-group">
      <li 
        class="list-group-item text-dark"
        *ngFor="let billing of option.providerLocBillingInfo"
        >
        {{billing.payeeName}}, {{billing.billingAddress1}}, {{billing.billingAddress2}}, {{billing.billingCity}}, {{billing.billingCounty}}, {{billing.billingStateCode}}, {{billing.billingCounty}}, {{billing.billingZip}}
      </li>
    </ul>
  </div>
</ng-template>

 <ng-container *ngIf="customToolTip; else noCustomLabel">
           <div 
            *ngFor="let option of filteredOptions" 
            class="p-2 bg-light dropdown-item">
              <label 
                [ngbTooltip]="toolTipTemplate"
                tooltipClass="custom-tooltip-class"
                #tooltip="ngbTooltip"
                (mouseenter)="toggleTooltipWithSelectedOption(tooltip,option)"
                container="body"
                >
                <input
                  type="checkbox" 
                  id="{{option.id}}"
                  [(ngModel)]="option.selected" 
                  (change)="toggleOptionSelection(option)"/>
                {{ option.label }}
              </label>
           </div>
         </ng-container>
inputValidation(){
    if(!this.inputOptions){
      // throw Error("The input options(inputOptions) for MultiselectDropdownComponent cannot be empty!!");
    }
    if(this.customToolTip && !this.toolTipTemplate){
      throw Error("When customTooltip is set true, a toolTipTemplate should also be passed to component");
    }
  }
