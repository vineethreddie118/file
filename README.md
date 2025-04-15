isTaxonomyCodeSelected(taxonomyList: ProviderTaxonomy[], code: string, currentIndex: number): boolean {
  return taxonomyList.some((t, idx) => t.taxonomyCode === code && idx !== currentIndex);
}
