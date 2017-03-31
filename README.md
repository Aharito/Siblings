# DLSiblings: Краткая документация
### Вывод соседних ресурсов с шаблонизацией (множественная кольцевая перелинковка)
Попробуем сделать Вики. Ё-моё, ну и морока оказалась с непривычки...

## Параметры сниппета:

1\) **&Qty** (целое), кол-во предыдущих и следующих соседей. Если &Qty=\`3\`, то общее кол-во будет 6 - то есть 3 перед и 3 после. Значение по умолчанию **2**.

2\) **&ownerTPL**, шаблон-обертка аналогично DocLister, но плейсхолдер НЕ [+dl.wrap+], а [+wrap+]. Значение по умолчанию  `@CODE:[+wrap+]`


Остальные параметры сниппета унаследованы от DocLister (такие же, как у него): шаблоны, условия выборки, остальные параметры.

### Параметры-исключения:
- режим **api** принудительно устанавливается в 1 (нет смысла задавать в сниппете)
- режим **debug** устанавливается в 0 (также нет смысла задавать).
- параметр **&display** принудительно устанавливается в &display=\`all\`, так как за кол-во отвечает параметр **&Qty**.

### ВНИМАНИЕ:
!!! В шаблонах сниппета **плейсхолдеры** ТВ-параметров записываются НЕ через точку, а через нижнее подчеркивание. То есть, будет **не** [+tv.h1+], а [+tv_h1+]. То же самое касается **экстендеров**, например экстендера e: вместо [+e.title+] пишем [+e_title+].

В общем, в плейсхолдерах шаблонов (только в них!) все точки **меняем** на нижнее подчеркивание (см. примеры вызова сниппета ниже).

## ПРИМЕРЫ

1\) Простой вызов сниппета.

	[[DLSiblings?
		&idType=`parents`
		&parents=`[*parent*]`
		&tpl=`@CODE: <a href="[+url+]">[+tv_h1+]</a><br>`
		&Qty=`2`
		&tvList=`h1`
	]]


2\) Более сложный пример с prepare и превьюшками FastImage

	[[DLSiblings?
		&idType=`parents`
		&parents=`[*parent*]`
		
		&thumbSnippet=`sgThumb`
		&thumbOptions=`{"tv.article_intro_img":{"small":"280x160","medium":"700x400"}}`   //Здесь НЕ надо менять точку на подчеркивание (это не шаблон)
		
		&ownerTPL=`@CODE:<div class="row latest-news inline-showcase">[+wrap+]</div><hr>`										
		&tpl=`@CODE:
		<div class="column col-1-1-2-4-4 margin-bottom-30">   //А здесь везде надо менять (это шаблон)              
			<div class="article-announce-img">
				<img class="img-responsive" src="[+tv_article_intro_img_medium+]" alt="[+e_title+] | [(cfg_company_brand_name)]">				
			</div>

			<div class="article-announce-content">
				<div class="article-announce-header">
					<a href="[+url+]">[+tv_h1+]</a>
				</div>
			</div>

			<a href="[+url+]" class="wrapper-link"></a>
		</div>`
		&Qty=`2`
		&tvList=`article_intro_img,h1`
		&prepare=`FastImagePreviews`
	]]
**Результат работы примера 2\):**

![image](https://cloud.githubusercontent.com/assets/6253807/24011145/8132a304-0aac-11e7-9ab9-2e1368cb0647.png)

**Примечание:** Это только примеры. Для того, чтобы они заработали на вашем сайте, у вас должны быть такие же TV и такие же prepare-сниппеты.

**Недостатки:**
- Длинная JSON-строка, содержащая все поля, в том числе контент (расход памяти)
- Формирование доп. индексного массива $ids (время)
- В результате сниппет подойдет для не очень больших выборок (навскидку до 1-2 тыс. ресурсов), для больших сайтов требуется другое решение
