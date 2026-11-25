# Inkling 2 Mini on Kaggle with Strata

A Kaggle notebook that runs Inkling 2 Mini with [Strata](https://github.com/Niko1221/Strata) and exposes it as an API protected by an API key.

## Requirements

- Kaggle account with Internet turned on
- GPU accelerator: T4 x2

## How to use

1. Upload `inkling2_mini_strata.ipynb` to Kaggle.
2. Turn on Internet and set the accelerator to GPU T4 x2.
3. Run **Install Strata and cloudflared** once.
4. In **Start the model**, replace `YOUR_SECRET_KEY` with your own key and run it once. The first run downloads the model.
5. Run **Test the model** to check that it answers.

The public address (a `trycloudflare.com` link) is printed when the model starts. Run **Show the address and key** if you lose it.

## Model variants

Change `MODEL=sq-10` in the launch cell to switch variant.

| Name | Size | Notes |
|---|---|---|
| `sq-10` | 10.47 GB | Default. Fits one T4. |
| `sq-15` | 14.35 GB | Better quality. Tight on one T4. |
| `sq-20` | 19.35 GB | Best quality. Needs both T4s. |

The `ud-*` variants (Unsloth files) currently fail Strata's file size check, so use the `sq-*` ones.

## Calling the API

```bash
curl https://YOUR-TUNNEL-URL/v1/chat/completions \
  -H "Authorization: Bearer YOUR_SECRET_KEY" \
  -H "Content-Type: application/json" \
  -d '{"messages": [{"role": "user", "content": "Say hello in one short sentence."}], "max_tokens": 256}'
```

The server speaks the OpenAI chat completions format, so it works with any OpenAI-compatible client.

## Notes

- Use a long random API key. Anyone with the link and the key can use your model.
- The tunnel link changes every time you start the model.
- The model does not support reasoning mode in Strata. It answers directly.
- Default context is 32k tokens. Input and output together must stay under that.
- Kaggle wipes the disk when the session ends, so run the install cell again and the model downloads again.
- Before restarting the model, run the **Stop** cell. It stops both Strata and the tunnel.
