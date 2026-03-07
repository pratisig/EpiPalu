# Patch Notes

## Fix NameError df_clim - Tab1 Dashboard

### Problème
`NameError: name 'df_clim' is not defined` à la ligne ~1497

### Cause
La variable `df_clim` était définie UNIQUEMENT dans le bloc :
```python
if st.session_state.df_climate_aggregated is not None:
    df_clim = st.session_state.df_climate_aggregated.copy()
```

Mais la **pyramide des âges** et les **graphiques d'évolution hebdomadaire** qui utilisaient `df_clim` (notamment dans `col_graph2`) se trouvaient DANS ce même bloc — mais avec une indentation incorrecte qui faisait qu'ils s'exécutaient parfois HORS du bloc.

### Correction
- Déplacer le bloc **Pyramide des âges** en dehors du bloc climat (il dépend de `dfpopulation`, pas du climat)
- Déplacer le bloc **Évolution Hebdomadaire** en dehors du bloc climat
- Dans `col_graph2`, ajouter une vérification `if st.session_state.df_climate_aggregated is not None:` avant d'utiliser `df_clim`
- `df_clim` n'est plus jamais référencé hors de son bloc de définition
