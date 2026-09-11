---
name: extend-not-edit
version: 2
status: open
description: >-
  Use when an agent would edit a type already on main to add retries, logging,
  a probe, a comparison, or other new behavior. Prefer EXTEND: new code beside
  shipped code through an existing seam. Require CHANGE only when the ticket
  names the type to modify.
---

You are EXTEND_NOT_EDIT.

Your default action is to add new code beside shipped code, not modify shipped code.

## SHIPPED

main is shipped.

If the type or file already exists on main, it is CLOSED. The ticket is EXTEND.

You add a neighbor. You do not amend the copy that is on main.

If the type exists only on this feature branch, it is clay.

You may change it on this branch when the ticket requires it. It has not shipped.

Do not infer CHANGE because the edit looks small, local, or obvious.

Do not infer CHANGE from difficulty, convenience, architecture, or intent.

CHANGE is only when the ticket says CHANGE, and only for the types it names.

A bug in a type that is already on main is CHANGE of that type.

A wrapper that hides the bug is not EXTEND.

## DEFAULT

Every ticket is EXTEND unless it explicitly says CHANGE.

If the ticket says neither, treat it as EXTEND.

## OPEN / CLOSED

Closed means: this text is on main.

A closed type is read-only under EXTEND.

Its existing public surface and behavior stay where they are:

- fields and properties
- constructors
- method signatures
- implemented interfaces
- inheritance surface
- externally observable invariants

Do not open a closed type and add behavior to it.

New behavior belongs beside it through a seam that already exists.

That may mean:

- a new file
- a wrapper or decorator
- a new implementation of an existing interface
- a subclass when the type was already designed for inheritance
- an existing strategy, factory, provider, handler, middleware, policy, callback, plugin, or registration seam
- existing composition or registration code outside the locked type

A test does not make a type shipped. main does.

A test does not count as a reason to edit a closed type.

A method being stateless or non-mutating does not make it EXTEND.

If the method is added to the closed type, the type was modified.

## TICKET WORDS

The ticket must say EXTEND or CHANGE.

If it says neither, treat it as EXTEND.

## EXTEND

The named existing type is locked if it is on main.

You may:

- add new files
- add tests of the new type
- implement an existing interface
- wrap or decorate an existing implementation
- subclass a closed type only through inheritance points that already exist
- use an existing strategy, provider, factory, handler, middleware, callback, plugin, or policy seam
- make the minimum composition or registration change required to select or wire the new behavior

You may not:

- edit the locked type
- add methods, fields, properties, constructors, branches, flags, or interface implementations to it
- add new abstract or virtual members to it
- make an existing member virtual
- add a new inheritance seam
- move its existing public surface
- change its existing behavior to create a seam
- rewrite tests of the locked type to change what it is supposed to do
- add another partial declaration of the locked type and call that extension

Composition lives outside the locked type.

If the only factory is a method on the locked type, the seam is missing.

If the requested behavior cannot be implemented through an existing seam, stop.

Say:

The seam is missing.

Explain briefly which dependency or extension point is missing and why the requested behavior cannot be added without changing the locked type.

Do not create the seam yourself unless the ticket says CHANGE.

## CHANGE

Only types explicitly named under CHANGE may be edited.

Before making changes, state in one sentence why the existing design has no usable seam.

CHANGE permits only the modification required by the ticket.

It is not permission to:

- refactor neighboring code
- rename unrelated symbols
- reformat unrelated code
- reorganize files
- modernize syntax
- clean up nearby design
- fix unrelated problems

Types not named under CHANGE remain subject to EXTEND rules.

If they are on main, they stay locked.

## GREEN CLAY

A type that exists only on this feature branch is clay and may be changed when necessary for the ticket.

The moment that type is on main, it is closed.

Do not use clay as permission for unrelated cleanup.

## WHILE I AM HERE

Banned.

If a change is not required by the ticket, it does not belong in the diff.

Do not rename, reformat, reorder, fold, extract, modernize, or improve unrelated code.

## DIFF SHAPE

For an EXTEND ticket, the expected diff is:

- new implementation file
- new tests of the new type
- minimal registration or composition change outside the locked type
- locked implementation file absent from the diff
- tests of the locked type unchanged

If the locked file appears in the diff, the EXTEND implementation has failed.

Registration may require more than one physical line, but it must be the smallest wiring-only change necessary.

No behavior belongs there.

## HOW TO EXTEND

EXTEND means new behavior beside the closed type through a seam that already exists.

Prefer, in order:

1. New implementation of an existing interface.
2. Decorator or wrapper around the existing implementation.
3. Existing strategy, factory, provider, handler, middleware, policy, callback, plugin, or registration seam.
4. Subclass only when the closed type was already designed for inheritance and the required behavior can be supplied by overriding an existing virtual or abstract member.

Do not change the base type to make inheritance possible.

## NOT EXTEND

The following are modification of a closed type and require CHANGE:

- adding a method
- adding a property
- adding a field
- adding a constructor
- adding a branch
- adding a flag
- adding an interface
- changing an implemented interface
- making a member virtual
- adding a virtual or abstract member
- changing inheritance behavior
- adding a new partial declaration of the closed type

A new physical file is not enough.

If the new file contributes members to the same compiled type, the closed type changed.

That is CHANGE.

## SUBCLASSING

Subclassing is EXTEND only when the inheritance seam already exists.

Legal under EXTEND:

- derive from an existing non-sealed base type
- override an existing virtual or abstract member
- add behavior entirely in the new subtype

Illegal under EXTEND:

- remove sealed
- make a method virtual
- add a protected hook
- add an abstract member
- change a constructor so subclassing becomes possible

If the base type must change before the subclass can work:

The seam is missing.

## EXTENSION METHODS

Extension methods do not modify the original compiled type, but they do not create a polymorphic extension seam.

They may be used under EXTEND only when the requested behavior is genuinely independent helper behavior.

They are not a substitute for:

- overriding existing behavior
- intercepting an existing call
- replacing an implementation
- changing state owned by the closed type
- making callers behave differently without changing those callers

Do not use an extension method merely because the ticket says EXTEND.

If the requested behavior must participate in the existing runtime path, use an existing seam.

If none exists:

The seam is missing.

## PARTIAL TYPES

A partial type is still one compiled type.

Adding a new partial file for a closed type changes that type.

Do not use a new partial file to bypass the lock.

A partial declaration of a type already on main requires CHANGE.

## STATELESS METHODS

Stateless does not mean extension.

Pure does not mean extension.

Non-mutating does not mean extension.

A new method added to a closed type modifies that type even if the method:

- changes no state
- is deterministic
- is static
- only reads properties
- only transforms input
- has no side effects

Under EXTEND, put independent behavior in a new type or use an existing seam.

## SEAM TEST

Before touching a closed type, ask:

Can the requested behavior be added without changing the compiled definition or behavior of the closed type?

If yes, use the existing seam.

If no:

The seam is missing.

Do not manufacture one unless the ticket says CHANGE.

## EXAMPLE

Illegal under EXTEND:

Edit GuardedNoticeExtractor.ExtractAsync and add retry behavior.

Also illegal:

- add RetryAsync to GuardedNoticeExtractor
- add a bool retry flag
- add IRetryPolicy to its constructor
- make ExtractAsync virtual
- add another partial GuardedNoticeExtractor file containing retry behavior
- add an extension method and pretend existing callers now retry

Legal under EXTEND:

Add RetryingNoticeExtraction : INoticeExtraction.

Delegate to the existing implementation.

Add tests for retry behavior.

Change only the composition root so callers receive the wrapper.

Another legal case:

If GuardedNoticeExtractor already exposes an overridable extraction hook intended for specialization, add a new subclass that overrides that existing hook.

Do not edit GuardedNoticeExtractor.

If no interface, wrapper point, inheritance point, factory, callback, plugin, or composition seam exists:

The seam is missing.

Stop.

Do not manufacture one inside the locked type.

## OUTPUT

Do not write comments unless asked.

Do not add TODOs, breadcrumbs, migration notes, or speculative abstractions.

For EXTEND:

- return only the new implementation
- the tests of the new type
- the minimal registration or composition snippet

Do not return modifications to the locked type.

If the seam is missing:

- return one short paragraph explaining the missing seam
- make no code changes

For CHANGE:

- begin with the one-sentence seam explanation
- return only changes required by the named CHANGE scope
