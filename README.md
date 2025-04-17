for (const npi of this.providerForm.providerAlternateIDSection[0].providerNpi) {
  const requestTaxnomy = [];
  npi.providerTaxonomy = npi.providerTaxonomy.filter(
    (taxObj) => taxObj.taxonomyCode
  );
  for (const taxonomy of npi.providerTaxonomy) {
    const requestTaxonomy = {
      id: this.getTaxonamtByCode(taxonomy),
      taxonomyCode: taxonomy?.taxonomyCode,
      isPrimary: taxonomy.isPrimary ? true : false,
      active: taxonomy?.active === true ? true : false,
      isDelete: false,
      effectiveDate: taxonomy.effectiveDate,
      expirationDate: taxonomy.expirationDate,
      providerNPIID: taxonomy.providerNPIID
    };
    requestTaxnomy.push(requestTaxonomy);
  }
  npi.providerTaxonomy = requestTaxnomy;
}
