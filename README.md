# ADA-Object-Detection

Object detection just for ADA &lt;3

### Przygotowanie środowiska

#### Pobrać submoduły

Jeśli sklonowano repo za pomocą `git clone`:

```shell
git submodule update --init
```

Jeśli repo pobrano jako zip, należy również pobrać wszystkie submoduły i wkleić do folderu z tym repo (np. yolov5).

#### Utworzyć i aktywować wirtualne środowisko

```shell
python -m venv .venv
source .venv/bin/activate
```

#### Zainstalować paczki

```shell
pip install --upgrade pip
pip install -r requirements.txt
pip install -r yolov5/requirements.txt
```

#### Uruchomić jupyter-lab lub kontynuować we własnym edytorze IDE

```shell
jupyter-lab
```

### Przygotowanie datasetu dla YOLO

#### Przygotowanie plików i ścieżek

* Przygotować ścieżkę do pliku `.csv` w następującym formacie (lub podobnym):

`datasets/test_db/annotations.csv`

```csv
filename,width,height,label,xmin,ymin,xmax,ymax
0.jpg,416,416,black-bishop,280,227,310,284
0.jpg,416,416,black-king,311,110,345,195
...
1.jpg,416,416,black-bishop,298,38,318,93
1.jpg,416,416,black-bishop,207,109,227,163
...
```

Nazwy kolumn i ich kolejność nie ma znaczenia, ale trzeba pamiętać, aby edytować skrypt konwertujący i użyć w nim
prawidłowych nazw kolumn.

* Przygotować ścieżkę do folderu z obrazami (folder musi zawierać tylko pliki graficzne):

`datasets/test_db/images`

* Przygotować plik `classes.names` z nazwami klas (plik powinien zawierać wszystkie unikatowe klasy zawarte w
  pliku `.csv`):

`datasets/test_db/classes.names`

```text
black-bishop
black-king
black-knight
...
white-queen
white-rook
```

* Przygotować ścieżkę, w której zostanie zapisany przekonwertowany dataset.

`datasets/test_db_converted`

#### Konwersja z formatu CSV na format YOLO

Po uruchomieniu skryptu konwertującego `.csv` na `yolo` (komórki `In 51` oraz `In 53` w notatniku), powinniśmy uzyskać
następujący wynik:

```text
datasets
├── test_db
│   ├── annotations.csv
│   ├── classes.names
│   └── images
│       ├── 0.jpg
│       ├── 1.jpg
│       ├── 2.jpg
│       ├── 3.jpg
│       ├── 4.jpg
│       ├── 5.jpg
│       └── 6.jpg
└── test_db_converted
    ├── images
    │   ├── 0.jpg
    │   ├── 1.jpg
    │   ├── 2.jpg
    │   ├── 3.jpg
    │   ├── 4.jpg
    │   ├── 5.jpg
    │   └── 6.jpg
    └── labels
        ├── 0.txt
        ├── 1.txt
        ├── 2.txt
        ├── 3.txt
        ├── 4.txt
        ├── 5.txt
        └── 6.txt
```

Powinien zostać utworzony folder z wybraną w punkcie wyżej nazwą (np. `test_db_converted`) a w nim dwa foldery: `images`
zawierający obrazy oraz `labels` zawi.


#### Przygotowanie configu

W folderze `config` znajduje się przykładowy plik `my_data.yaml`.

```yaml
# Train/val/test sets as 1) dir: path/to/imgs, 2) file: path/to/imgs.txt, or 3) list: [path/to/imgs1, path/to/imgs2, ..]
path: ../datasets/test_db_converted  # dataset root dir
train: images/train  # train images (relative to 'path')
val: images/val  # val images (relative to 'path')
test: images/test  # test images (optional)

# Classes
nc: 12  # number of classes
names: [ 'black-bishop',
         'black-king',
         'black-knight',
         'black-pawn',
         'black-queen',
         'black-rook',
         'white-bishop',
         'white-king',
         'white-knight',
         'white-pawn',
         'white-queen',
         'white-rook' ]  # class names
```

zmienna `names` powinna być listą wszsytkich nazw klas, z dokładnie taką samą kolejnością jak w pliku `classes.names`.