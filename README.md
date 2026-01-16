# PJ Cloud Portfolio (S3 + CloudFront + GitHub Actions)

Static portfolio website hosted on **Amazon S3** and delivered globally with **CloudFront**.  
Deployments are automated using **GitHub Actions** (CI/CD) with an IAM user that has least-privilege permissions.

## Live Site
- CloudFront URL: https://d192ivfc2s4l4d.cloudfront.net

## Architecture
- **S3**: Stores website files (private origin)
- **CloudFront**: CDN in front of S3 for global caching + HTTPS
- **IAM**: `github-deploy` user with limited permissions
- **GitHub Actions**: On push to `main`, syncs files to S3 and invalidates CloudFront

## CI/CD Workflow (What happens on every push)
1. GitHub Actions runs on `push` to `main`
2. Configures AWS credentials using repository secrets
3. Syncs repo files to the S3 bucket (`aws s3 sync`)
4. Invalidates CloudFront cache so updates appear quickly

## GitHub Secrets Used
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_REGION`
- `S3_BUCKET`
- `CLOUDFRONT_DISTRIBUTION_ID`

## Tech Used
AWS (S3, CloudFront, IAM), GitHub Actions, HTML/CSS

## Notes / Lessons Learned
- CloudFront is a **global** service; invalidation permissions must be granted via IAM.
- Correct CloudFront Distribution ID is required to avoid `NoSuchDistribution`.
