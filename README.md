if (err.status === 500 && err.errors && err.errors.length > 0) {
        const backendErrorMessage = err.errors.join('\n'); // Join multiple errors if needed
        this.addService.fnRaiseWarningDlg(
          'Error',
          backendErrorMessage // Show the exact error message received from the backend
        );
        return; // Stop further execution and do not navigate to the next screen
      }
