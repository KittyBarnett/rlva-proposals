# Request for Comments (RFC): Shared folder wear/unwear notifications

**Author(s):** Kitty Barnett  
**Date:** 2025-02-11  
**Status:** Draft  
**Related RFCs:** n/a

---

## 1. Summary

Shared folders can be attached and detached in various ways, but RLVa currently doesn’t provide any follow-up notification when a requested folder’s items have actually been worn or removed.

## 2. Motivation

A script may need to attach a folder and then take further action only after its contents have successfully attached to (or been removed from) the avatar. While polling options exist (see below), they introduce unnecessary complexity. A built-in notification would significantly simplify this process and make scripting more reliable.

## 3. Specification

### 3.1 Command/Restriction Overview

- **Command/Restriction Name:** `@notify`
- **Syntax:** `@notify:<channel>[;needle]=<n|y>` (existing command)
- **Parameters:** Refer to @notify documentation.
- **Example Usage:**  

    ```plaintext
    @notify:1234;inv_folder=add,attach:My/Test/Folder=force
    ```
    Expected results:
    * `/worn inv_folder My/Test/Folder`  
    * `/unworn inv_folder My/Test/Folder`
    * `/partially_worn inv_folder My/Test/Folder` [To Discuss]
    * `/partially_unworn inv_folder My/Test/Folder` [To Discuss]
    * `/blocked inv_folder My/Test/Folder` [To Discuss]

### 3.2 Behavior Details

RLVa will track items queued for wearing or removal and maintain a pending list. Once all items are processed, it will send a notification.

* If restrictions prevent wearing/removal of certain items, the notification will still trigger when all **filtered** pending items are resolved.
* **To Discuss**: Differentiate between full and partial wears (similar logic as @getinvworn).
* Edge cases: If the folder is already fully worn or nothing is worn, a success notification should still be sent.
* **To Discuss**: If none of the items can be worn or removed due to restrictions, RLVa could optionally send a blocked notification.

## 4. Impact Analysis

### 4.1 Compatibility

- Will this change break any existing behavior?  No, `inv_folder` isn't currently used as a keyword in the existing notifications.
- If so, how can the transition be handled smoothly? n/a

### 4.2 Security and Privacy Considerations

- Are there potential exploits or abuse scenarios? No.
- Does this affect user privacy in any way? Minimal. `@notify` already allows tracking of the initial attach command — this simply extends that to confirm completion.

### 4.3 Performance Considerations

- Does this introduce any performance bottlenecks? No.
- Any expected impact on resource consumption? No.

## 5. Alternatives Considered

Polling the avatar’s attachment list in LSL or repeatedly calling `@getinvworn` could achieve similar results, but these methods are unreliable and cumbersome.

Also, I want this implemented, so I declare all workaround options to be 'Not Good Enough™' 😛.

## 6. Open Questions

#### Partial worn/unworn

Would distinguishing between full and partial wears be useful? If so, this needs to be implemented from the start rather than added later.

#### Timeouts

What happens if a folder is requested to be attached, but none of its items are actually worn (e.g. '*Asset missing from database*' for the oldbies)?

Should the script be responsible for handling this with its own timeout?
Or should RLVa send a timeout notification after a set period? If so, how long should that timeout be?

## 7. References

n/a

---

**(End of RFC)**
