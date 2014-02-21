JTSSmartPunctuation
===================

A drop-in utility for adding as-you-type smart punctuation to a UITextView. For iOS 7 or later.

## Smart What?

JTSSmartPunctuation replaces common shorthands composed of dumb punctuation with their smart counterparts. It turns dumb quotes into **smart quotes**, three consecutive periods into an **elipsis**, three consecutive dashes into an **em-dash**, and two consecutive dashes followed by anything but a dash with an **en-dash**. It’s compatible with **right-to-left languages**, and safe to use with composed character sequences, like **emoji**. It only scans the immediate vicinity around recent edits, so it should perform well even with very long runs of text.


## Usage

On iOS 7 or later, a text input view like `UITextView` uses an instance of `NSTextStorage` as its backing store for the text. This text storage object takes an optional delegate that conforms to the `NSTextStorageDelegate` protocol. This protocol includes the following method:

```objc
- (void)textStorage:(NSTextStorage *)textStorage 
 willProcessEditing:(NSTextStorageEditActions)editedMask 
              range:(NSRange)editedRange 
     changeInLength:(NSInteger)delta;
```

This method is called *after* the user has performed a new edit to the text, but *before* that edit has been committed and the UI updated. Thus it is the perfect time to replace dumb punctuation with smart punctuation. 

Your application will need an object that sets itself as the delegate for the text storage of your text view:

```objc
[self.textView.textStorage setDelegate:self];
```

Then implement the following delegate method:

```objc
- (void)textStorage:(NSTextStorage *)textStorage
 willProcessEditing:(NSTextStorageEditActions)editedMask
              range:(NSRange)editedRange
     changeInLength:(NSInteger)delta {
    
    [JTSSmartPunctuation fixDumbPunctuation:textStorage
                                editedRange:editedRange
                            textInputObject:self.textView];
}
```

That's it.
