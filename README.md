<div>
    <div align="centre">
        <img alt='Weaviate logo' src='https://weaviate.io/img/site/weaviate-logo-light.png' width='240' align='centre' />
    </div>
    <br>
    <br>
    <br>
</div>

# Weaviate Anime Search Engine
Welcome to the Weaviate Anime Search Engine! This anime search engine is powered by a deep neural network using Weaviate's vector database. It utilizes **ResNet50 (PyTorch)** as the vectorizer and retriever. Please note that currently, only PyTorch ResNet50 supports **CUDA-GPU**, but in a **single-threaded** manner. You can also use the **keras-based ResNet50 CPU** with **multi-threaded** inference.

![image](https://github.com/Sravanthgithub/anime-search-weaviate/assets/77894804/35110b5b-9946-4f6f-a242-c58e10d47ca4)


## How to Run
Follow these steps to run the anime search engine:

1. Install Docker (skip this if already installed)
    ```sh
    curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh
    ```

2. Spin up the Docker Compose file:
    ```sh
    docker compose up -d
    ```

3. Activate the environment:
    ```sh
    pip3 install pipenv && pipenv install && pipenv shell
    ```

4. Launch the `start.sh` script to create the Weaviate schema, convert animes/images from the `img` folder to base64, and upload them to Weaviate:
    ```sh
    ./start.sh
    ```

5. Navigate to the URL that Streamlit outputs and upload an anime/image. The program will query Weaviate for a vectorized search of similar images. If you are running this in a local environment, the URL will be:
    ```
    http://127.0.0.1:8501/
    ```

6. (Optional) To handle CORS and XSRF protection in Streamlit, run this command if you encounter **Socket Errors** while connecting to the Streamlit app URL:
    ```
    streamlit run streamlit_app.py --server.enableCORS false --server.enableXsrfProtection false
    ```

## Images
Feel free to insert your animes/images into the `img` folder and see if the database suggests similar images.

## Weaviate Schema
This is the schema implemented in Weaviate. You can modify the class name, change the vectorizer, or add more properties as needed.
```python
schemaConfig = {
    'class': 'MyImages', # class name for schema config in Weaviate (change it with a custom name for your images)
    'vectorizer': 'img2vec-neural',
    'vectorIndexType': 'hnsw',
    'moduleConfig': {
        'img2vec-neural': {
            'imageFields': [
                'image'
            ]
        }
    },
    'properties': [
        {
            'name': 'image',
            'dataType': ['blob']
        },
        {
            'name': 'text',
            'dataType': ['string']
        }
    ]
}


