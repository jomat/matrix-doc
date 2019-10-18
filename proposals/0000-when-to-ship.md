# MSC0000: Facilitating early releases of software dependent on spec

*Note*: This is a process change MSC, not a change to the spec itself.

There's currently an unanswered question by the spec process: when is it
safe to start using stable endpoints or to present a feature as "stable"?
Historically this question would receive very different answers depending
on who you asked, so in an effort to come up with a concise answer the
following process change is proposed.

## Proposal

The new process, from start to finish, is proposed as:

1. Have an idea for a feature.
2. Optionally: implement the feature using unstable endpoints, vendor prefixes,
   and unstable feature flags as appropriate.
   * When using unstable endpoints, they MUST include a vendor prefix. For
     example: `/_matrix/client/unstable/com.example/login`. Vendor prefixes
     throughout this proposal always use the Java package naming convention.
   * Unstable endpoints **do not** inherit from stable (`/r0`) APIs.
   * When using an unstable feature flag, they MUST include a vendor prefix.
     This kind of flag shows up in `/versions`. Eg: `com.example.new_login`.
   * You can ship the feature at *any* time, so long as you are able to accept
     the technical debt. The implementation MUST support the flag disappearing
     and be generally safe for users. Note that implementations early in the MSC
     review process may also be required to provide backwards compatibility with
     earlier editions of the proposal.
   * If you don't/can't support the technical debt, do not implement the feature
     and wait for Step 7.
   * If at any point the idea changes, the feature flag should also change so
     that implementations can adapt as needed.
3. In parallel, or ahead of implementation, open an MSC and solicit review.
4. Before an FCP (Final Comment Period) can be called, the Spec Core Team will
   require that evidence to prove the MSC works be presented. A typical example
   of this is an implementation of the MSC (ie: Step 2).
5. FCP is gone through, and assuming nothing is flagged the MSC lands.
6. A spec PR is written to incorporate the changes into Matrix.
7. A spec release happens.
8. Implementations switch to using stable prefixes (ie: `/r0`) if the server
   supports the specification version released. If the server doesn't advertise
   the specification version, but does have the feature flag, unstable prefixes
   should still be used.
9. A transition period of about 2 months since the spec release happens before
   implementations start to encourage other implementations to switch to stable
   endpoints. For example, the Synapse team should start asking the Riot team to
   support the stable endpoints (as per Step 8) 2 months after the spec release.

It's worth repeating that this process generally only applies if the implementation
wants to ship the feature ahead of the spec being available. By doing so, it takes
on the risk that the spec/MSC may change and it must adapt. If the implementation
is unable to take on that risk, or simply doesn't mind waiting, it should go through
the normal spec process.

To help MSCs get incorporated by implementations as stable features, the spec core
team plans to release the specification more often. How often is undefined and is
largely a case-by-case basis.

To reiterate:

* Implementations MUST NOT use stable endpoints before the MSC is in the spec. This
  includes NOT using stable endpoints post-FCP.
* Implementations CAN ship features that are exposed by default to users before an
  MSC has been merged to the spec, provided they follow the process above.
* Implementations SHOULD be wary of the technical debt they are incuring by moving
  faster than the spec.