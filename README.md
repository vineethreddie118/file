    // Manually trigger Angular change detection
    this.model.update.emit(input.value);

@HostListener('paste', ['$event'])
  onPaste(event: ClipboardEvent) {
    event.preventDefault();
    const pastedValue = event.clipboardData?.getData('text') || '';

    const cleanedVal = pastedValue.replace(/[-\/]/g, '');

    if (cleanedVal.length !== 8) return;

    let trimmed = cleanedVal.replace(/\s+/g, '');

    if (trimmed.length > 10) {
      trimmed = trimmed.substr(0, 10);
    }

    trimmed = trimmed.replace(/-/g, '');

    let numbers = [];
    numbers.push(trimmed.substr(0, 2));
    if (trimmed.substr(2, 2) !== '') numbers.push(trimmed.substr(2, 2));
    if (trimmed.substr(4, 4) != '') numbers.push(trimmed.substr(4, 4));

    const formattedValue = numbers.join('/');

    this.el.nativeElement.value = formattedValue;
    
    // Dispatch an input event to update the form model
    this.el.nativeElement.dispatchEvent(new Event('input', { bubbles: true }));

    // Manually update Angular's model binding
    setTimeout(() => {
      this.model.update.emit(formattedValue);
    });
  }
