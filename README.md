getFilteredTaxonomyCodes(taxonomyList: ProviderTaxonomy[], currentCode: string): any[] {
  const selectedCodes = taxonomyList
    .map(t => t.taxonomyCode)
    .filter(code => code && code !== currentCode); // keep the current row's selection

  return this.taxonomyCodes.filter(tc => !selectedCodes.includes(tc.taxonomyCode) || tc.taxonomyCode === currentCode);
}


ng-container *ngFor="let providerType of getFilteredTaxonomyCodes(npiAt.providerTaxonomy, taxonomy.taxonomyCode)"
