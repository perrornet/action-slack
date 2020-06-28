---
title: Custom use case
metaTitle: Custom use case | action-slack
metaDescription: This describes the custom use case of action-slack.
---

You will often want to send notifications in a format other than the one that action-slack has determined.  
In such a case, consider using `type: custom`.

If you specify a payload in accordance with the slack specification, action-slack will notify you as it is.  
Use `status: cutom` and [custom_payload](/with#custom_payload) to customize notifications to your liking can be sent.

```yaml
steps:
  - uses: 8398a7/action-slack@v3
    with:
      status: custom
      custom_payload: |
        {
          text: "Custom Field Check",
          attachments: [{
            "author_name": "8398a7@action-slack", // json
            fallback: 'fallback',
            color: 'good',
            title: 'CI Result',
            text: 'Succeeded',
            fields: [{
              title: 'lower case',
              value: 'LOWER CASE CHECK'.toLowerCase(),
              short: true
            },
            {
              title: 'reverse',
              value: 'gnirts esrever'.split('').reverse().join(''),
              short: true
            },
            {
              title: 'long title1',
              value: 'long value1',
              short: false
            }],
            actions: [{
            }]
          }]
        }
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # optional
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }} # required
```

As you can see, the JavaScript functionality is available in the custom_payload . (e.g. `toLowerCase()`)  
We have even more options for those who want to use custom notifications, but want to use the Fields feature.

```yaml
steps:
  - uses: 8398a7/action-slack@v3
    with:
      status: custom
      fields: workflow,job,commit,repo,ref,author,took
      custom_payload: |
        {
          username: 'action-slack',
          icon_emoji: ':octocat:',
          attachments: [{
            color: '${{ job.status }}' === 'success' ? 'good' : '${{ job.status }}' === 'failure' ? 'danger' : 'warning',
            text: `${process.env.AS_WORKFLOW}\n${process.env.AS_JOB} (${process.env.AS_COMMIT}) of ${process.env.AS_REPO}@${process.env.AS_REF} by ${process.env.AS_AUTHOR} ${{ job.status }} in ${process.env.AS_TOOK}`,
          }]
        }
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

You can access the values retrieved by Fields through environment variables.  
See [Fields](/fields) for the available environment variables.

<img width="510" alt="custom" src="https://user-images.githubusercontent.com/8043276/85949706-44926f80-b993-11ea-83bf-b3f81e5e2d8c.png" />

If there's a good format, I'd like to introduce it on this page from time to time.