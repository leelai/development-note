# iOS app

## Initial view controller from storyboard

```
let storyboard = UIStoryboard(name: "myStoryboardName", bundle: nil)
let vc = storyboard.instantiateViewController(withIdentifier: "myVCID")
self.present(vc, animated: true)
```