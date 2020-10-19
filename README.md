# Data Science com Python - Recomendação de Filmes
Primeiro projeto nessa área e mexendo com CSV em *python*, algumas pontos no código não são os ideais ou performático, foi feito para fins de aprendizagem.

A base que utilizei se encontra no [Kaggle](https://www.kaggle.com/rounakbanik/the-movies-dataset), também me inspirei/aprendi em alguns dos seus notebooks.

## Dados

Utilizei o arquivo *movies_metadata.csv* para pegar todas informações que utilizarei.
Algumas informações como *year*, *adult*, *genres*, foi utilizado apenas para fins de mostrar os dados em forma de gráfico (como disse anteriormente, esse projeto era para aprender e ter primeiros contatos com gráficos no [pandas](https://pandas.pydata.org/)).

Já a recomendação, utilizei com a formula do **IMDb** que encontrei nesse [link](https://www.quora.com/How-does-IMDbs-rating-system-work) e também encontrei neste [notebook](https://www.kaggle.com/ibtesama/getting-started-with-a-movie-recommendation-system) no qual me ajudou em alguns momentos.

### Gráficos

![Genres](https://i.imgur.com/1IftxMT.png)

![Adults](https://i.imgur.com/OHfUVX9.png)

![Runtime](https://i.imgur.com/nG4oy2X.png)

![Year](https://i.imgur.com/lJKNkSF.png)
Esse especifico não é a melhor maneira, vi outras maneiras de tratar durante o processo e estudos *(utilizando o próprio valor Period)*, mas não me apeguei tanto agora.

## Recomendação

Aqui é onde acontece a recomendação.

	#IMDb's rating system - formula 
	#weighted rating (WR) = (v ÷ (v+m)) × R + (m ÷ (v+m)) × C
	#Where:
	#R = average for the movie (mean) = (Rating)
	#v = number of votes for the movie = (votes)
	#m = minimum votes required to be listed in the Top 250 (currently 25,000)
	#C = the mean vote across the whole report
	def get_vote_average():
	    all_vote_count = movies_metadata['vote_count'].tolist()
	    all_vote_count = [n for n in all_vote_count if str(n) != 'nan']
	    m = sum(all_vote_count) / len(all_vote_count) * .7
	    
	    all_vote_average = movies_metadata['vote_average'].tolist()
	    all_vote_average = [n for n in all_vote_average if str(n) != 'nan']
	    c = sum(all_vote_average) / len(all_vote_average)
	    
	    scores = []
	    for index, row in movies_metadata.iterrows():
	        v = row['vote_count']
	        R = row['vote_average']
	        weighted_rating = (v/(v+m) * R + (m/(m+v) * c))
	        scores.append(weighted_rating)
	    
	    return scores

	movies_metadata['score'] = get_vote_average()
	movies_metadata = movies_metadata.sort_values('score', ascending=False)
	movies_metadata.head(5)

	movies_metadata['score'] = get_vote_average()
	movies_metadata = movies_metadata.sort_values('score', ascending=False)
	movies_metadata.head(5)

Outro ponto que pode ser visto no código é a performance, como é a primeira vez que leio *csv* e programo com *python* com essa finalidade, segui focando primeiramente no resultado.

Percorrendo toda o arquivo data, vou adicionando um novo campo chamado *score* onde será utilizado para fazer a recomendação. Pois utilizar apenas o *vote_average* poderia trazer filmes com apenas 1 recomendação, justamente procurando que achei esse calculo do IMDb, no qual eles utilizam no [IMDb Top 250 TV](https://www.imdb.com/chart/toptv/)

Após adicionar a nova coluna no data eu faço um ordeno a lista com a coluna *score*, trazendo as 5 primeiras da lista.

### Observações
No inicio do código removi as colunas que não utilizaria:

    movies_metadata = pd.read_csv('movies_metadata.csv')

	def delete_columns():
	    del movies_metadata["belongs_to_collection"]
	    del movies_metadata["budget"]
	    del movies_metadata["homepage"]
	    del movies_metadata["id"]
	    del movies_metadata["imdb_id"]
	    del movies_metadata["original_language"]
	    del movies_metadata["overview"]
	    del movies_metadata["popularity"]
	    del movies_metadata["poster_path"]
	    del movies_metadata["production_companies"]
	    del movies_metadata["production_countries"]
	    del movies_metadata["revenue"]
	    del movies_metadata["spoken_languages"]
	    del movies_metadata["status"]
	    del movies_metadata["tagline"]
	    del movies_metadata["video"]
	    
	delete_columns()
Entretanto é possível puxar na hora de apresentar apenas os campos que são desejáveis, não precisando remover cada um destes, no qual geraria um código melhor e mais elegante. Mas para testes e para *debugar*, remover os dados me facilitou a enxergar os dados de forma mais prática.

## Resultado
Pode ser encontrado neste [link.](https://github.com/GivagoFritzen/python-recommendation-movies/blob/main/Movie%20analise.ipynb)
