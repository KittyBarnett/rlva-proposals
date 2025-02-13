# Request for Comments (RFC): RLVa enable/disable toggle changes

**Author(s):** Kitty Barnett  
**Date:** 2025-02-12  
**Status:** Draft  
**Related RFCs:** n/a

---

## 1. Summary

Currently, RLVa can be toggled on and off at the login screen without requiring a viewer restart. This proposal aims to refine and improve that functionality.

## 2. Motivation

Originally, toggling RLVa required a full viewer restart. To mitigate the impact of XtremRLV, I modified it so that RLVa could be toggled at the login screen, but not while logged in.

However, there's no technical reason why RLVa couldn't be toggled on or off while logged in — aside from the need for careful implementation and handling within the viewer code.

## 3. Specification

### 3.1 Command/Restriction Overview

- **Command/Restriction Name:** n/a
- **Syntax:** n/a
- **Parameters:** n/a
- **Example Usage:**  n/a

### 3.2 Behavior Details

Toggling RLVa on or off while logged in will still only be permitted under very specific conditions. 

The toggle will be allowed only if no currently worn attachment has issued an RLV command. For example, if an attachment issued @version at login and has not issued any further commands within three hours, toggling will still be blocked. However, if the user detaches it, then they can toggle to their heart's content.

To enhance user experience, it will likely be helpful to provide a list of attachments responsible for blocking the toggle in the "error" message.

## 4. Impact Analysis

### 4.1 Compatibility

- Will this change break any existing behavior?  No. If an attachment has issued an RLV command, toggling will still not be allowed.
- If so, how can the transition be handled smoothly? n/a

### 4.2 Security and Privacy Considerations

- Are there potential exploits or abuse scenarios? Probably, if I get the logic wrong.
- Does this affect user privacy in any way? No.

### 4.3 Performance Considerations

- Does this introduce any performance bottlenecks? No.
- Any expected impact on resource consumption? No.

## 5. Alternatives Considered

None.

## 6. Open Questions

Is this actually needed or wanted?

## 7. References

n/a

---

**(End of RFC)**
