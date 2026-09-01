My screen-shaped endpoint would be GET /study-room-booking. It should be split into real resources such as rooms and bookings. These are real things in the system that can be identified and managed independently.

## Self-Check

### 1.

I would reject POST /v1/orders/{id}/markReady because the URI uses a verb instead of a noun and does not properly represent the transition. I would use POST /v1/orders/{id}/readiness instead.

### 2.

No, it is not read-only. Yes, it is safe to repeat because PUT makes the resource match the given data.

### 3.

Use 409 Conflict because the request is valid but cannot be completed due to the current business state. 500 is wrong because it means a server failure, while 200 is wrong because the request did not succeed.

### 4.

The dangerous operation is POST /v1/bookings because repeating it could create a duplicate booking for the same room and time slot. The user could lose the availability of the room or end up with an unwanted duplicate booking. Idempotency-Key is a version-4 UUID with hyphens and is required on POST /v1/bookings. It is ignored everywhere else. Keys are kept for 24 hours, and a key reused after that is treated as new. 409 Conflict with type https://api.example.com/problems/idempotency-key-reuse is returned if the same key is used with different contents.

### 5.

I understand how an idempotency key prevents duplicate bookings, but I am still unsure how the server should handle the same key when the original request is still being processed.

PASS