# UI review checklist

Use this as a fast scan. The goal is to find the biggest usability wins with minimal change.

## Information hierarchy

- Is there a clear page title that matches user intent?
- Is the primary action obvious within 3 seconds?
- Are secondary actions visually secondary?
- Are sections grouped logically with consistent spacing?

## Visual design

- Typography: readable sizes (esp. mobile), consistent headings, sane line length
- Color: sufficient contrast, consistent semantic colors (success/warn/error)
- Spacing: consistent rhythm (8px-based is fine), no cramped clusters
- Alignment: consistent edges and baselines

## Forms

- Labels are persistent (do not rely on placeholder as label)
- Clear required/optional markers
- Inline validation is helpful and not noisy
- Error messages explain how to fix the issue
- Submit button disabled only when necessary; explain why

## States

- Loading: skeleton/progress, prevents double-submit
- Empty: explains what to do next
- Error: provides recovery path
- Success: confirms outcome and next step

## Navigation & wayfinding

- Current location is clear
- Back/close behavior is predictable
- Breadcrumbs/tabs have good labels

## Microcopy

- Buttons are verb-first and specific ("Save changes" vs "Submit")
- Avoid jargon; match user language
- Confirmations are short and unambiguous
