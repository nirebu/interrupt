# #!/interrupt

In questo repository caricherò gli script usati per i video del canale [interrupt](https://www.youtube.com/@interrupt-0x21). Alcuni saranno fatti per essere utilizzati con [presenterm](https://github.com/mfontanini/presenterm), altri saranno dei semplici file markdown

## Presenterm

### Usare le speaker notes
Gli script fatti per presenterm sono pensati per essere lanciati in duplice maniera:
- pubblicare le speaker notes
- ascoltare le speaker notes

```bash
# in un terminale
presenterm --publish-speaker-notes presentazione.md
# in un secondo terminale
presenterm --listen-speaker-notes presentazione.md
```

### Eseguire codice nelle presentazioni
Per abilitare l'esecuzione del codice all'interno delle presentazioni:
```bash
presenterm -x presentazione.md
```
> [!CAUTION]
> Non eseguite codice senza sapere esattamente cosa sta facendo.

E poi utilizzare <kbd>ctrl+e</kbd> per eseguirlo