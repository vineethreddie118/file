<ng-container *ngFor="let providerType of getFilteredTaxonomyCodes(npiAt.providerTaxonomy)">
getFilteredTaxonomyCodes(selectedTaxonomies: ProviderTaxonomy[]): any[] {
  const selectedCodes = selectedTaxonomies.map(t => t.taxonomyCode);
  return this.taxonomyCodes.filter(tc => !selectedCodes.includes(tc.taxonomyCode));
}
