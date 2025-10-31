# IBDesignable-Guide
This guide will describe the use of IBDesignable

# 🧩 Using @IBDesignable and @IBInspectable in Swift

## 🧠 Overview
`@IBDesignable` and `@IBInspectable` are used in Swift to make custom UI components visually editable inside **Interface Builder (Storyboard/XIB)**.

---

## ✅ Example: Custom DesignableTextField

```swift
@IBDesignable
class DesignableTextField: UITextField {
    
    @IBInspectable var cornerRadius: CGFloat = 8.0 {
        didSet { updateView() }
    }
    
    @IBInspectable var borderWidth: CGFloat = 1.0 {
        didSet { updateView() }
    }
    
    @IBInspectable var borderColor: UIColor = .lightGray {
        didSet { updateView() }
    }
    
    @IBInspectable var padding: CGFloat = 10.0
    
    override func awakeFromNib() {
        super.awakeFromNib()
        updateView()
    }
    
    override func prepareForInterfaceBuilder() {
        super.prepareForInterfaceBuilder()
        updateView()
    }
    
    private func updateView() {
        layer.cornerRadius = cornerRadius
        layer.borderWidth = borderWidth
        layer.borderColor = borderColor.cgColor
    }
    
    override func textRect(forBounds bounds: CGRect) -> CGRect {
        return bounds.insetBy(dx: padding, dy: 0)
    }
    
    override func editingRect(forBounds bounds: CGRect) -> CGRect {
        return bounds.insetBy(dx: padding, dy: 0)
    }
    
    override func placeholderRect(forBounds bounds: CGRect) -> CGRect {
        return bounds.insetBy(dx: padding, dy: 0)
    }
}
```

---

## 💬 Why Default Values Don’t Show in Storyboard

Even though you set default values in Swift (like `cornerRadius = 8.0`), **Interface Builder does not apply them visually** until you manually edit them in the property inspector.

This happens because Storyboard only **serializes** properties that are changed manually. Default Swift values aren’t serialized by default.

### ✅ Fix: Add `updateView()` calls in
- `awakeFromNib()`
- `prepareForInterfaceBuilder()`

That ensures your default values show both **at runtime** and **in Interface Builder preview**.

---

## 🧱 Useful @IBDesignable Components (for future use)

### 🌟 DesignableButton
```swift
@IBDesignable
class DesignableButton: UIButton {
    @IBInspectable var cornerRadius: CGFloat = 8.0 { didSet { layer.cornerRadius = cornerRadius } }
    @IBInspectable var borderWidth: CGFloat = 1.0 { didSet { layer.borderWidth = borderWidth } }
    @IBInspectable var borderColor: UIColor = .blue { didSet { layer.borderColor = borderColor.cgColor } }
}
```

### 💠 DesignableView
```swift
@IBDesignable
class DesignableView: UIView {
    @IBInspectable var cornerRadius: CGFloat = 10.0 { didSet { layer.cornerRadius = cornerRadius } }
    @IBInspectable var shadowColor: UIColor = .black { didSet { layer.shadowColor = shadowColor.cgColor } }
    @IBInspectable var shadowOpacity: Float = 0.3 { didSet { layer.shadowOpacity = shadowOpacity } }
    @IBInspectable var shadowOffset: CGSize = CGSize(width: 2, height: 2) { didSet { layer.shadowOffset = shadowOffset } }
}
```

### ✏️ DesignableImageView
```swift
@IBDesignable
class DesignableImageView: UIImageView {
    @IBInspectable var cornerRadius: CGFloat = 12.0 { didSet { layer.cornerRadius = cornerRadius; clipsToBounds = true } }
    @IBInspectable var borderWidth: CGFloat = 0.5 { didSet { layer.borderWidth = borderWidth } }
    @IBInspectable var borderColor: UIColor = .gray { didSet { layer.borderColor = borderColor.cgColor } }
}
```

---

## ⚙️ Pros and Cons

| ✅ Pros | ⚠️ Cons |
|--------|---------|
| Makes UI editable directly in Storyboard | May slightly slow storyboard rendering |
| Reduces code repetition | Not ideal for highly dynamic components |
| Preview UI instantly | Large projects may lag in IB preview |
