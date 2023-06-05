# AI Developer Tools


```python
from langchain.embeddings import OpenAIEmbeddings
from openai.error import RateLimitError
form openai_tools import OpenAICredentialManager

emb_db = [] # a list to save 
text_loader = ...# is a generator that yields cleaned texts
# create a credential manager object
cred_man = OpenAICredentialManager("./openai.apikey")
cm = iter(cred_man)
key, nickname = next(cm)
# pick caption text one by one
for caption in text_loader:
    while True:
        model = OpenAIEmbeddings(openai_api_key=key, model="ada", max_retries=1)
        try:
            if cred_man.is_limit_exhausted(nickname):
                raise RateLimitError("Rate limit exhausted for {}".format(nickname))
            
            # single
            embedding = model.embed_query(caption)
            emb_db.append(caption, embedding)
            # time.sleep(60 / rpm)
            break
        except RateLimitError:
            cred_man.set_limit_exhausted(nickname)
            key, nickname = next(cm)
```