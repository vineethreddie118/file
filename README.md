addNpiRow() {
    const providerNpi = new ProviderNpi();
    providerNpi.providerTaxonomy = [];
    this.providerAlternateIDSection[0].providerNpi.push(providerNpi);
  }
