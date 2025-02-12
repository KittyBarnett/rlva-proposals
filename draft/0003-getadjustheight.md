# Request for Comments (RFC): @getadjustheight

**Author(s):** Kitty Barnett  
**Date:** 2025-02-12  
**Status:** Draft  
**Related RFCs:** n/a

---

## 1. Summary

Introduce a reply command that returns the avatar's current height offset.

## 2. Motivation

Currently, scripts have no reliable way to determine the original hover height. For example, if a script plays an animation and adjusts the avatar’s height using `@adjustheight`, it has no means of restoring the original hover height once the animation ends. Most scripts attempt a best-effort restoration by issuing `@adjustheight:1;0;0=force`, but this is far from ideal.

## 3. Specification

### 3.1 Command/Restriction Overview

- **Command/Restriction Name:** `@getadjustheight`
- **Syntax:** `@getadjustheight=<channel>`
- **Parameters:** None.
- **Example Usage:**  

    ```plaintext
    @getadjustheight=0
    ```
    Expected results:
    * `<distance_pelvis_to_foot_in_meters>;<factor>[;delta_in_meters]` (same format as @adjustheight input parameters)

### 3.2 Behavior Details

Returns the avatar's current height offset in the @adjustheight format. If the most recent hover height change was due to an RLV command, it will return that command’s option. Otherwise, it will return the user’s manually set hover height in the correct format (e.g., 1;0;<offset>).

## 4. Impact Analysis

### 4.1 Compatibility

- Will this change break any existing behavior?  No.
- If so, how can the transition be handled smoothly? n/a

### 4.2 Security and Privacy Considerations

- Are there potential exploits or abuse scenarios? No.
- Does this affect user privacy in any way? No.

### 4.3 Performance Considerations

- Does this introduce any performance bottlenecks? No.
- Any expected impact on resource consumption? No.

## 5. Alternatives Considered

None.

## 6. Open Questions

#### Optionless @adjustheight variation

While @getadjustheight provides a way for scripts to retrieve the current hover height, there may be a more streamlined approach to restoring hover height after modification.

A possible alternative would be allowing an optionless @adjustheight=force to revert to the previous hover height before the last @adjustheight change.

Before:
- @getadjustheight=0 (Output: 1;0;0.123)
- @adjustheight:1;0;0.456=force
- @adjustheight:1;0;0.123=force

After:
- @adjustheight:1;0;0.456=force (RLVa internally tracks 1;0;0.123 as the current offset)
- @adjustheight=force (RLVa restores the previous offset by issuing @adjustheight:1;0;0.123=force internally)

This eliminates the need for a separate command listener and removes the coordination delay between issuing `@getadjustheight` and applying `@adjustheight`.

However, this approach raises several considerations:
- User sets their hover height to X
- Script plays an animation with hover height Y
- User adjusts the hover height to a more perfect Z
- Script stops the animation and issues @adjustheight=force

In this case, we would want the hover height to return to X, which is what it was before the script overrode the user's preference. Is this always the correct approach?

Handling Multiple Scripts: What happens if multiple scripts are modifying hover height simultaneously?

Poorly Scripted Objects: Some scripts may issue @adjustheight=force indiscriminately, potentially causing unintended behavior. Should there be safeguards against this?

Successive Calls to @adjustheight=force: What should happen if @adjustheight=force is issued multiple times in succession?

## 7. References

n/a

---

**(End of RFC)**
