 addTaxonomyRow(index: number) {
    const taxonamyObj = new ProviderTaxonomy();
    this.providerAlternateIDSection[0].providerNpi[index].providerTaxonomy.push(
      taxonamyObj
    );
    
  }

<button type="button" class="btn btn-primaryicon btn-sm m-1 ml-4" (click)="addTaxonomyRow(idx)" data-toggle="tooltip"
              title="Add Taxonomy">
        <i class="fa-solid fa-plus"></i> Add Taxonomy
      </button>
      <div style="margin-left: 20px;">
        <div class="form-row form_field_outer_row p-1"
             *ngFor="let taxonomy of npiAt.providerTaxonomy; let i = index">
          <div class="col-md-6 col-lg-4 col-xl-3">
            <label for="inputData">Taxonomy Code</label>
            <select class="form-control form-control-custom" id="Taxonomy{{i}}" name="Taxonomy{{i}}"
                    #Taxonomy="ngModel" [(ngModel)]="taxonomy.taxonomyCode" (change)="handleTaxonomyCodeChange($event, i)">
              <option value=""></option>
              <ng-container *ngFor="let providerType of taxonomyCodes;let idx= index"  >
                <option [ngValue]="providerType.taxonomyCode" [disabled]="providerType.isDisabled" >
                  {{providerType.taxonomyCode}}
                </option>
              </ng-container>
            </select>
          </div>
