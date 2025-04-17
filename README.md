for (const npi of this.providerForm.providerAlternateIDSection[0].providerNpi) {
  npi.providerTaxonomy = npi.providerTaxonomy
    .filter((taxObj) => taxObj.taxonomyCode)
    .map((taxonomy) => {
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
      return requestTaxonomy;
    });
}
