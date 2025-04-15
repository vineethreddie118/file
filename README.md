<ng-container *ngFor="let providerType of taxonomyCodes">
  <option [ngValue]="providerType.taxonomyCode"
          [disabled]="isTaxonomyCodeSelected(providerType.taxonomyCode, i)">
    {{ providerType.taxonomyCode }}
  </option>
</ng-container>

isTaxonomyCodeSelected(code: string, currentIndex: number): boolean {
  return this.npiAt.providerTaxonomy.some((t, idx) => t.taxonomyCode === code && idx !== currentIndex);
}
