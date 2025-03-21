const expControl = this.practiceform.controls['expirationDate'];

if (expControl && (!expControl.value || Object.keys(expControl.value).length === 0)) {
  expControl.setErrors(null); // ✅ Clear both invalidDate and ngbDate errors
}
