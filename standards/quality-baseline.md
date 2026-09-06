# Quality Baseline

This seed is a starting point that should grow and change as real projects are built. It offers guidance, not a mandate: **default does not mean mandatory**.

The chef considers global defaults first, then project-type defaults, project-specific requirements, and finally exceptions or overrides. Each later level takes precedence over the levels before it.

## User-facing interfaces (candidates)

- Responsive design across relevant screen sizes
- Accessibility and semantic HTML
- Keyboard navigation and visible focus states
- `prefers-reduced-motion` support and sensible transitions
- Loading, empty, error, and success states
- Consistent typography, spacing, and design tokens
- Form validation and good touch behavior
- Performance-conscious implementation and optimized assets
- ...and similar concerns

## Public websites (additional candidates)

- SEO and Open Graph metadata
- Semantic heading structure
- A sitemap
- A suitable `robots.txt`
- Structured data where appropriate
- Useful 404 handling
- ...and similar concerns

## Applications (candidates)

- Environment variable handling
- Input validation
- Authentication and authorization considerations
- Secure secret handling
- Error handling
- Rate limiting where appropriate
- Useful logging
- Testing, type-checking, and linting where appropriate
- ...and similar concerns

None of these are automatically required; the chef selects what fits the project, and the user can override that selection.
