# Request for Comments (RFC): Add shared folder exceptions to a subset of behaviours

**Author(s):** Kitty Barnett (on behalf of Marissa)  
**Date:** 2025-02-14  
**Status:** Draft  
**Related RFCs:** n/a

---

## 1. Summary

This proposal extends certain attachment interaction behaviors to allow referencing the target by shared folder path rather than relying solely on UUIDs.

## 2. Motivation

The requester wanted the ability to attach a shared folder while restricting user interactions—specifically preventing touches on those attachments. The initial request was for @touchthis, but if implemented, this change should also apply to other relevant commands, including:
  - @edit
  - @editobj
  - @editattach (which currently lacks an exception test)
  - @showhovertext
  - @touchthis
  - @touchattachself
  - @touchhud

## 3. Specification

### 3.1 Command/Restriction Overview

- **Command/Restriction Name:** `@touchthis`
- **Syntax:** `@touchthis:path/to/folder=n|y`
- **Parameters:** Shared folder path.
- **Example Usage:**  

    ```plaintext
    @touchthis:TestFolder=n
    ```
    Expected results:
    * Any attachment within TestFolder should not be touchable.

### 3.2 Behavior Details

The existing behavior and validation checks for affected commands remain unchanged—this simply introduces a new way to define exceptions.

## 4. Impact Analysis

### 4.1 Compatibility

- Will this change break any existing behavior?  No.
- If so, how can the transition be handled smoothly? n/a

### 4.2 Security and Privacy Considerations

- Are there potential exploits or abuse scenarios? No.
- Does this affect user privacy in any way? No.

### 4.3 Performance Considerations

- Does this introduce any performance bottlenecks? Exception lookups are currently efficient, so care should be taken to ensure that adding inventory folder checks does not negatively impact performance.
- Any expected impact on resource consumption? No.

## 5. Alternatives Considered

An alternative approach would involve using LSL scripts to:
  - Attach a folder.
  - Wait for the attachment to complete (see Proposal #2).
  - Use llGetAttachedList to retrieve object UUIDs.
However, in this case, the object in question is a HUD, and HUD objects are not returned by LSL functions, making this approach unworkable.

Moreover, allowing path-based exceptions simplifies scripting significantly since the viewer would track objects across attach/detach cycles without requiring manual intervention.

A separate request was made for a command that would return all worn attachments (including their UUIDs) in various categories—such as all attachments, HUD-only attachments, or shared inventory attachments. However, this is a non-starter for me. RLVa will not expose any information beyond what is already accessible via LSL functions.

## 6. Open Questions

Are there any additional commands that should be included in this change?

## 7. References

n/a

---

**(End of RFC)**
