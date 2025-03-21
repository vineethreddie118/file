console.log('Is form valid?', this.practiceform.valid);
Object.keys(this.practiceform.controls).forEach(key => {
  const control = this.practiceform.controls[key];
  console.log(`Control: ${key}, Valid: ${control.valid}, Errors:`, control.errors, 'Value:', control.value);
});
