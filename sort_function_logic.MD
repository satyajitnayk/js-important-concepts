**Logic Behind .sort()**

```typescript
import {PDFDocument} from 'pdf-lib';

const fields = pdfDocument
  .getForm()
  .getFields()
  .sort((f1, f2) => {
    // sort by y coordinate value(y value more means the text is on top)
    const y1 = f1.acroField.getWidgets()[0].getRectangle().y;
    const y2 = f2.acroField.getWidgets()[0].getRectangle().y;
    return y1 < y2 ? 1 : y1 > y2 ? -1 : 0;
  });
```

In the provided code, the sorting logic within the `.sort()` function uses the values `1` and `-1` to determine the
order of elements during the sorting process. The significance of these values lies in their ability to influence the
sorting order of elements in an array.

When using the `.sort()` function, the comparison function is expected to return specific values to instruct how
elements should be ordered relative to each other. Here's what these values mean:

1. Returning `1`:
   When the comparison function returns `1`, it indicates that the first element (`f1`) should be considered greater
   than the second element (`f2`) during the sorting process. As a result, `f1` will be placed after `f2` in the sorted
   array.

2. Returning `-1`:
   On the other hand, when the comparison function returns `-1`, it signifies that the first element (`f1`) should be
   considered less than the second element (`f2`) during the sorting process. Consequently, `f1` will be placed
   before `f2` in the sorted array.

These values are used by the sorting algorithm to determine the relative ordering of elements based on the comparison
function's logic. By returning `1` or `-1`, the sorting algorithm knows how to rearrange the elements in the desired
order.

In the specific context of the code provided, the comparison function returns `1` when `y1` (Y-coordinate of the first
widget in `f1`) is less than `y2` (Y-coordinate of the first widget in `f2`). This means that form fields with higher
Y-coordinates (closer to the top of the page) will be placed after form fields with lower Y-coordinates (closer to the
bottom of the page) in the sorted array.

Likewise, the comparison function returns `-1` when `y1` is greater than `y2`. This ensures that form fields with lower
Y-coordinates will be placed before form fields with higher Y-coordinates in the sorted array.

By using these values in the comparison function, the form fields are sorted in descending order based on their
Y-coordinate values, with the form fields closer to the top of the page appearing first in the sorted array.
