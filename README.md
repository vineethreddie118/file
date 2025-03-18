 public fnIsUPDInsertSuccess(errorObj: any): number {
    let nUPDId: number = 0;
    for(let i=0;i <errorObj?.error?.errors?.length; i++) {
      if(errorObj?.error?.errors[i].includes("UPD: Insert Provider Information isSuccess")) {
        const splitResult = errorObj?.error?.errors[i].split('|');
        if(splitResult.length > 1) {
          nUPDId = splitResult[1];
          break;
        }
      }
    }
    return nUPDId;
  }
