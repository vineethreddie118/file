this.providerForm.providerAlternateIDSection.forEach((altIdSection) => {
  altIdSection.providerNpi.forEach((npi) => {
    npi.providerTaxonomy.forEach((taxonomy) => {
      if (taxonomy.taxonomyCode) {
        taxonomy['isDisabled'] = true;
      }
