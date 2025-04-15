getFilteredTaxonomyCodes(currentIndex: number) {
  const selectedCodes = this.npiAt.providerTaxonomy
    .filter((_, i) => i !== currentIndex)
    .map(t => t.taxonomyCode);
  
  return this.taxonomyCodes.filter(tc => !selectedCodes.includes(tc.taxonomyCode));
}
