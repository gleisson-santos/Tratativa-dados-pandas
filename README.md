# Tratativa-dados-pandas
Tratativas de codigo utilizando o Pandas para correção e melhoria de analise

```python
from numpy import dtype
import pandas as pd
from IPython.display import display
import matplotlib.pyplot as plt
from IPython.display import display
from PIL.EpsImagePlugin import split
import datetime
import time
import re
```

```python
#Import tabelas de dados e prazos
idg_df = pd.read_excel('264.xlsx')
prazos_df = pd.read_excel('Prazos ISAP.xlsx')
idg_df
```
![image](https://user-images.githubusercontent.com/33934341/195919376-3ca1f1d3-6ff3-4dd6-889b-428b442fb115.png)


```python
#Alterar a localidade do bairro em especifico
idg_df.loc[idg_df['Bairro'] == '22 - JD MARGARIDAS', 'Localidade'] = '700 - LAURO DE FREITAS'
idg_df[['Bairro', 'Localidade']]
```
![image](https://user-images.githubusercontent.com/33934341/195919413-b7143f70-772a-42fa-8c29-f3acaa659c1a.png)

```python
#Criando coluna e concatenando de ID
idc1 = idg_df['IDC1'] = idg_df['Tipo'].map(lambda x : x.split(' - ')[0])
IDC2 = idg_df['IDC2'] = idg_df['Especificação'].map(lambda x : x.split(' - ')[0])
idg_df['ID_PRAZO'] = idg_df['IDC1'].map(str) + '.' + idg_df['IDC2'].map(str)
idg_df = idg_df.merge(prazos_df[['Item Contratual', 'Prazo Atendimento', 'ID_PRAZO']], on='ID_PRAZO')
idg_df
```
![image](https://user-images.githubusercontent.com/33934341/195919462-b185156c-4e71-4c18-bb73-528f1258e049.png)

```python
#Coluna de majoração
idg_df['Prazo Atendimento'] = idg_df['Prazo Atendimento'].astype('int64')
idg_df['Tempo de Atendimento'] = idg_df['Tempo de Atendimento'].astype('int64')

idg_df['Majoração'] = idg_df['Tempo de Atendimento'] / idg_df['Prazo Atendimento'].round()
idg_df['Majoração'] = idg_df['Majoração'].astype('float')
idg_df[['Tempo de Atendimento', 'Prazo Atendimento', 'Majoração']]
```

![image](https://user-images.githubusercontent.com/33934341/195919647-02018cb1-69d6-4097-bd0d-61656864a146.png)

