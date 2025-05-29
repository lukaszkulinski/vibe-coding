# Logging Guidelines (PHP/Symfony + Monolog)

## Philosophy

Logging is a critical part of building, operating, and maintaining robust software systems. Effective logging ensures:
- **Operational transparency** (what is happening in production)
- **Efficient debugging and monitoring**
- **Auditability and compliance** (who did what and when)
- **Security** (alerts, suspicious activity)

Log messages should be structured, actionable, and support both humans and automated tools in understanding application behavior.

---

## Core Principles

- **Clarity & Relevance**: Every log message must have a clear purpose. Avoid generic messages ("Something went wrong").
- **Consistency**: Use consistent message formats, logging levels, and contextual fields across the codebase.
- **Security & Privacy**: Never log sensitive or personally identifiable information (PII), such as passwords, card tokens, full emails, or raw request bodies.

---

## Logging Tools and Configuration

- Use **Monolog** (via `LoggerInterface`) as the only logging abstraction.
- Inject the logger using Symfony's autowiring wherever needed (controllers, services, commands).
- **Configure multiple channels/handlers** as required:
    - Separate channels for `app`, `security`, `api`, `payment`, `audit`, etc.
    - Handlers for file logging, external services (Sentry, Graylog, ELK), and emails on `critical` errors.
- In production, redirect logs to an external log management system (e.g., Sentry, Graylog, ELK/EFK).

---

## Logging Levels (Monolog)

Use logging levels appropriately:
- `emergency` – System is unusable.
- `alert` – Action must be taken immediately.
- `critical` – Critical conditions, unexpected failures.
- `error` – Runtime errors that do not require immediate action but should be monitored.
- `warning` – Exceptional occurrences that are not errors.
- `notice` – Normal but significant events.
- `info` – Informational messages (business logic, state changes).
- `debug` – Detailed debugging information.

**Choose the lowest sufficient level** for your message; do not overuse `error` or `critical`.

---

## What to Log

- **Startup/shutdown events** (app/server start, stop, reload)
- **Key business actions** (user registration, payments, external API calls)
- **Security events** (failed logins, permission denials, suspicious activity)
- **State changes** (status updates, entity changes)
- **External system interactions** (requests, responses, failures)
- **Errors, exceptions, and failed assertions** (with stack traces)
- **Audit events** (who/what/when for sensitive operations, e.g., payment status changes)
- **Performance metrics** (optional, e.g., request duration for critical endpoints)

---

## What NOT to Log

- Sensitive information (PII, passwords, card tokens, raw card numbers, raw authorization headers)
- Large or binary payloads (limit to summaries or IDs)
- Highly repetitive, redundant, or low-value messages (clutter)
- Data that could violate compliance requirements (e.g., GDPR)

---

## Contextual Logging

- Always use the **context array** to provide relevant metadata (`user_id`, `order_id`, `request_id`, etc.)
- For each log message, include:
    - The minimum required to uniquely identify the action (IDs, timestamps)
    - Additional details to help with debugging and auditing (request parameters, external references)
- For errors/exceptions, log:
    - Exception message, class, code, file, and stack trace (if applicable)
    - All available context (which operation, parameters, user/session info, correlation/request IDs)

---

## Structured Logging

- Use structured logging where possible (JSON logs, key-value context)
- Avoid putting all details into the free-text message; leverage Monolog's context for important fields

**Example:**
```php
$this->logger->info(
    'User completed payment.',
    [
        'user_id' => $userId,
        'order_id' => $orderId,
        'amount' => $amount,
        'method' => 'card',
    ]
);
```

---

## Error and Exception Logging

- Always log exceptions at `error` or higher.
- Log both the exception and any relevant business context.
- Never swallow exceptions without logging and justification.
- Use `@throws` in PHPDoc when a method can throw an exception, and document log points in complex flows.

**Example:**

```php
try {
    $result = $paymentService->charge($order, $cardToken);
} catch (PaymentException $e) {
    $this->logger->error(
        'Payment failed.',
        [
            'order_id' => $order->getId(),
            'user_id' => $order->getUser()->getId(),
            'exception' => [
                'message' => $e->getMessage(),
                'code' => $e->getCode(),
                'class' => get_class($e),
                'trace' => $e->getTraceAsString(),
            ]
        ]
    );
    throw $e;
}
```
---

## Channel and Handler Configuration

- **Separate logs** by channel: `app`, `security`, `api`, `payment`, `audit`, etc.
- For sensitive/audit logs, ensure limited access and strict retention.
- Configure Sentry handler for exception reporting (`error` and above).
- Use rotating file handlers to prevent disk overflow.

---

## Logging in Tests and Development

- Use `debug` or `info` level in tests; avoid cluttering test output with excessive logs.
- In development, log as much as needed for debugging, but ensure logs are not pushed to production.

---

## Log Message Best Practices

- Write log messages in English, using clear, precise language.
- Use present tense and active voice.
- Start with what happened (event), then add details.
- Make logs actionable (should make sense out of context).
- Never expose secrets, tokens, or PII in logs—mask or omit such values.
- Regularly review log messages for clarity and usefulness.

---

## Review & Maintenance

- All new code should be reviewed for logging quality.
- Refactor log statements when code changes or logic is updated.
- Remove outdated log messages and adjust logging levels as needed.
- Document logging channels, handlers, and conventions in the project README.

---

## Example: Good vs. Bad Logging

**Bad:**
```php
$this->logger->info('User did something');
```
**Good:**
```php
$this->logger->info(
    'User changed password.',
    [
        'user_id' => $userId,
        'ip' => $request->getClientIp(),
        // Do NOT include the old/new password!
    ]
);
```
---

## Compliance & Retention

- Log retention and access must comply with legal and company policies (e.g., GDPR).
- Sensitive/audit logs must be encrypted or secured at rest.
- Regularly review and rotate log files as required by policy.

---

## Summary

- Always log meaningful events using Monolog.
- Use the right level and context for every log message.
- Never log sensitive or personal data.
- Maintain log clarity, consistency, and security at all times.
