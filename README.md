if (
        this.providerForm.providerId === 0 &&
        err.errors &&
        err.errors.some((errorMsg) =>
          errorMsg.includes('Provider already exists with matching')
        )
      ) 
