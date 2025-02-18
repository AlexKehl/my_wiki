# Mailgun

An API that enables to send, receive, and track email.

## Send an email

Important to use the correct api url like here `api.eu.mailgun.net`.

```node
import { serverEnv } from "~/env";

export namespace Email {
  export async function send({
    subject,
    text,
  }: {
    subject: string;
    text: string;
  }): Promise<void> {
    const form = new FormData();
    form.append("from", `SomeSender <somesender@${serverEnv.MAILGUN_DOMAIN}>`);
    form.append("to", `<${serverEnv.FEEDBACK_EMAIL}>`);
    form.append("subject", subject);
    form.append("text", text);

    await fetch(
      `https://api.eu.mailgun.net/v3/${serverEnv.MAILGUN_DOMAIN}/messages`,
      {
        method: "POST",
        headers: {
          Authorization: "Basic " + btoa(`api:${serverEnv.MAILGUN_API_KEY}`),
        },
        body: form,
      }
    );
  }
}
```
