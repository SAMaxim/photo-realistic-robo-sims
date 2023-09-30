# Фотореалистичные симуляторы + датасеты для задачи IBVS 

Добрый день, утро, ночь! В качестве тестового задания был выдан файл с 4 пунктами, однако вроде как выполнить нужно было только первый пункт. Я не был уверен, что делать, поэтому постарался выполнить 2/4 пунктов (самые важные не получилось выполнить в следствие ограниченности производительности ПК). В этом репозитории вы найдёте:

1. Описания двух фотореалистичных симуляторов для построения сцен для обучения нейронной сети для задачи IBVS. Также есть предложение о плагинах, изменениях и технологиях, которые можно внести в эти open source проекты.

2. Датасеты с фотореалистичными 3D моделями бытовых предметов, пригодные для манипуляций роботом.

3. ~~Третий пункт, к сожалению, не был выполнен. (см. 4 пункт)~~

4. ~~Четвёртый пункт не был выполнен по той же причине, что и третий.~~

## 1. Фотореалистичные симуляторы:

В качестве фотореалистичных симуляторов были выбраны 2 open source проекта:

- [SPEAR: A Simulator for Photorealistic Embodied AI Research](https://github.com/isl-org/spear#building-from-source)

<img src="./images/SPEARSim_mainPage.jpeg" height="350" />

Данный симулятор разработала Intel Labs в сотрудничестве с Центром компьютерного зрения в Испании, Куджиале в Китае и Мюнхенским техническим университетом. Этот проект - фотореалистичная платформа моделирования с открытым исходным кодом, которая ускоряет обучение и валидацию различных систем искусственного интеллекта. Основная область данного проекта - симуляции в закрытых помещениях. Решение можно загрузить по [лицензии MIT](https://github.com/isl-org/spear/blob/main/LICENSE.txt) с открытым исходным кодом.

SPEAR можно запустить на macOS, Windows или Linux. Для работы необходим Unreal Engine 5.2, ссылка на который уже лежит в репозитории. Также есть подробная инструкция по установке, причём подробная для всех платформ.

- [UnrealROX](https://github.com/3dperceptionlab/unrealrox#unrealrox)

<img src="./images/UnrealROXSim_example1.jpeg" height="200" />
<img src="./images/UnrealROXSim_example2.jpeg" height="200" />
<img src="./images/UnrealROXSim_example3.jpeg" height="200" />
<img src="./images/UnrealROXSim_example4.jpeg" height="200" />

[UnrealROX](https://sim2realai.github.io/UnrealROX/) - это среда, построенная на движке Unreal Engine 4. Она описана в [этой статье](https://arxiv.org/pdf/1810.06936.pdf). Основным преимуществом данного симулятора является [подробная документация](https://unrealrox.readthedocs.io/en/latest/) и возможность "цеплять" камеру куда угодно, тем самым тренируя нейросети для различных конфигураций IBVS.

Небольшая сводная таблица о двух симуляторах:

|                               | SPEAR                                       | UnrealROX                                |
| ----------------------------- | ------------------------------------------- | ---------------------------------------- |
| Лицензия                      | MIT License                                 | MIT License                              |
| Движок                        | Unreal Engine 5.2                           | Unreal Engine 4                          |
| Поддержка VR                  | Нативной нет                                | Есть                                     |
| Предпочтительная конфигурация | Eye-to-hand / end-point closed-loop control | Обе конфигурации доступны "из коробки"   |
| Инструкция по установке       | [вот](https://github.com/isl-org/spear/blob/main/docs/building_spearsim.md) | [вот](https://unrealrox.readthedocs.io/en/latest/_site/project/installation.html#installation) |


Фотореализм важен для задачи IBVS. Но фотореализм требует большой производительности, и кол-во FPS обычно оставляет желать лучшего. Недавно вышла [статья](https://dl.acm.org/doi/pdf/10.1145/3592433), авторы которой смогли разработать эффективный алгоритм, позволяющий довольно быстро обучать и гораздо быстрее использовать 3D Gaussian Splatting. Подобная технология даёт поражающие [результаты](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/) в области фотореализма. Если необходимо отсканировать некое помещение и фотореалистично отобразить его, то это как раз нужная технология (отражения, полупрозрачные объекты получаются несравнимо лучше чем, например, у NeRF). Она прекрасно работает как раз внутри помещений (небо довольно сложно "нарисовать" с помощью неё, а вот потолок нет).

Оба описанных здесь симулятора построены на базе движка Unreal Engine, что позволяет легко создавать фотореалистичные сцены. Однако SPEAR от Intel построен на 5ой версии UE, к которой уже существует [плагин](https://www.unrealengine.com/marketplace/en-US/product/3d-gaussians-plugin) в маркетплейсе для упомянутой выше технологии. Очень советую обратить внимание на эту технологию. На [этом сайте](https://poly.cam/gaussian-splatting) можно поиграться.

## 2. Датасеты:

- [A Large Dataset of Object Scans (redwood-3dscan)](https://github.com/isl-org/redwood-3dscan)

<img src="./images/Redwood-3dscan.jpeg" height="200" />

Множество бесцветных объектов. Превью можно посмотреть [здесь](http://redwood-data.org/3dscan/).

| Размер | RGBD scans | Meshes | Категории |
| ------ | ---------- | ------ | --------- |
| ~4Tb   | 10,933     | 441    | 320       |

- [ShapeNet](https://www.shapenet.org/)

<img src="./images/ShapeNet.png" height="200" />

Огромное количество самых различных объектов. Есть пробный период, но так стоимость - $99. Превью моделей можно посмотреть [здесь](https://www.shapenet.org/model-querier).

| Размер      | Meshes | Категории |
| ----------- | ------ | --------- |
| очень много | 220000 | 3135      |

- [Household Objects for Pose Estimation (HOPE)](https://github.com/swtyree/hope-dataset)

<img src="./images/HOPE.jpg" height="200" />

28 3D-объектов от NVidia. Посмотреть полностью можно на ссылаемом выше github, но скачать все модели можно [тут](https://drive.google.com/drive/folders/1jiJS9KgcYAkfb8KJPp5MRlB0P11BStft).

| Размер      | Meshes | Категории |
| ----------- | ------ | --------- |
| ~170Mb      | 28     | 1         |

- [Amazon Berkeley Objects (ABO) Dataset](https://amazon-berkeley-objects.s3.amazonaws.com/index.html#explore)

<img src="./images/ABODataset.png" height="200" />

Датасет продуктов с Амазона. Большая часть объектов слишком большие для манипулятора. Чтобы посмотреть [превью](https://amazon-berkeley-objects.s3.amazonaws.com/index.html#explore) нужно активировать метадату.

| Размер      | Meshes | Категории |
| ----------- | ------ | --------- |
| 271 Gb      | ~7960  | много     |

- [HomebrewedDB: RGB-D Dataset for 6D Pose Estimation of 3D Objects](https://campar.in.tum.de/personal/ilic/homebreweddb/index.html)

<img src="./images/HomebrewedDB.png" height="200" />

Небольшой датасет (17 игрушек, 8 предметов домашнего обихода и 8 промышленных объектов).

| Размер      | Meshes | Категории |
| ----------- | ------ | --------- |
| неизвестно  | 33     | 3         |

- [OmniObject3D](https://omniobject3d.github.io/)

<img src="./images/Omniobject3d.gif" height="200" />

Большой датасет самых различных отсканированных объектов.

| Размер      | Meshes | Категории |
| ----------- | ------ | --------- |
| неизвестно  | 6000   | 190       |

- [Toys4K 3D Object Dataset](https://github.com/rehg-lab/lowshot-shapebias)

<img src="./images/Toy4k.gif" height="200" />

Довольно солидный датасет с игрушками.

| Размер      | Meshes | Категории |
| ----------- | ------ | --------- |
| неизвестно  | 4000   | 105       |