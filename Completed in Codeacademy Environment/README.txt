# Completed-Code-in-Learning-Environment
import codecademylib
import pandas as pd
from matplotlib import pyplot as plt
species=pd.read_csv('species_info.csv')
print species.head()
species_count=species.scientific_name.nunique()
species_type=species.category.unique()
conservation_statuses=species.conservation_status.unique()
conservation_counts=species.groupby('conservation_status').scientific_name.nunique().reset_index()
print conservation_counts
species.fillna('No Intervention', inplace = True)
conservation_counts_fixed=species.groupby('conservation_status').scientific_name.nunique().reset_index()
species = pd.read_csv('species_info.csv')

species.fillna('No Intervention', inplace = True)
protection_counts = species.groupby('conservation_status')\
    .scientific_name.nunique().reset_index()\
    .sort_values(by='scientific_name')
plt.figure(figsize=(10,4))
ax=plt.subplot()
plt.bar(range(len(protection_counts)),protection_counts.scientific_name.values)
ax.set_xticks(range(len(protection_counts)))
ax.set_xticklabels(protection_counts.conservation_status.values)
plt.ylabel('Number of Species')
plt.title('Conservation Status by Species')
plt.show()

species = pd.read_csv('species_info.csv')

species.fillna('No Intervention', inplace = True)
species['is_protected']= species.conservation_status !='No Intervention'
category_counts=species.groupby(['category','is_protected']).scientific_name.nunique().reset_index()
print category_counts.head()
category_pivot=category_counts.pivot(columns='is_protected',index='category',values='scientific_name').reset_index()
category_pivot.columns=['category','not_protected','protected']
category_pivot['percent_protected']=category_pivot.protected/(category_pivot.protected+category_pivot.not_protected)
print category_pivot
contingency=[[30,146],[75,413]]
contingency_2=[[30,146],[5,73]]
from scipy.stats import chi2_contingency
chi2, pval,dof, expected=chi2_contingency(contingency)
print pval
chi3,pval_reptile_mammal,dof2,expected2=chi2_contingency(contingency_2)
print pval_reptile_mammal

species = pd.read_csv('species_info.csv')
species.fillna('No Intervention', inplace = True)
species['is_protected'] = species.conservation_status != 'No Intervention'
observations=pd.read_csv('observations.csv')
print observations.head()
species['is_sheep']=species.common_names.apply(lambda common_names: 'Sheep' in common_names)
species_is_sheep=species[species.is_sheep]
print species_is_sheep
sheep_species=species[(species.is_sheep)&(species.category=='Mammal')]
print sheep_species
sheep_observations=observations.merge(sheep_species)
print sheep_observations.head()
obs_by_park=sheep_observations.groupby('park_name').observations.sum().reset_index()
print obs_by_park

species = pd.read_csv('species_info.csv')
species['is_sheep'] = species.common_names.apply(lambda x: 'Sheep' in x)
sheep_species = species[(species.is_sheep) & (species.category == 'Mammal')]

observations = pd.read_csv('observations.csv')

sheep_observations = observations.merge(sheep_species)

obs_by_park = sheep_observations.groupby('park_name').observations.sum().reset_index()
plt.figure(figsize=(16,4))
ax=plt.subplot()
plt.bar(range(len(obs_by_park)),obs_by_park.observations.values)
ax.set_xticks(range(len(obs_by_park)))
ax.set_xticklabels(obs_by_park.park_name.values)
plt.ylabel('Number of Observations')
plt.title('Observations of Sheep per Week')
plt.show()

baseline=15
minimum_detectable_effect=33
sample_size_per_variant=890
yellowstone_weeks_observing=1.755
bryce_weeks_observing=3.56