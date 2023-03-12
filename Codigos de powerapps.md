# *Codigo del compose donde se agrega la tabla de ideas por turnos.*

<table>
  <tr>
    <th>Turno</th>
    <th>Cantidad</th>
  </tr>
  @{variables('Turnos')}
    <td>Weekly</td>
    <td>@{length(outputs('Get_TotalIdeasWeekly')?['body/value'])}</td>
  <tr>
    <th>Total</th>
    <th>@{variables('TotalIdeas')}</th>
  </tr>
</table>

# *Codigo de busqueda de happy puntos cuando se buscaba total de happy puntos y happy puntos disponibles*

Concurrent(
    ClearCollect(
        puntos_ideas;
        Filter(
            IdeasFE;
            Codigo = txtemployee_1.Text
        )
    );
    ClearCollect(
        puntos_ideasTot;
        Filter(
            IdeasFE;
            Codigo = txtemployee_1.Text;
            Clasificación = "Canje" || Clasificación = "Idea de Mejora" || Clasificación = "Certificado" || Clasificación = "Safety Catch" || Clasificación = "Evento Kaizen"
        )
    )
);;
Concurrent(
    Set(
        asociadobuscando;
        LookUp(
            HC;
            CODIGO = txtemployee_1.Text
        )
    );
    Set(
        totalidisp;
        Sum(
            puntos_ideas;
            HappyPuntos
        ) - Sum(
            puntos_ideas;
            Canjeados
        )
    )
);;
Set(
    totali;
    Sum(
        puntos_ideasTot;
        HappyPuntos
    ) - Sum(
        puntos_ideasTot;
        Canjeados
    )
);;
UpdateContext({lblerroasociado: 1})

# *Anterior forma de consultar los happy puntos.*

UpdateContext({resettext: !resettext});
UpdateContext({resettext: !resettext});

Clear(colAllListHP);
With(
   {
      wSets: 
      With(
         {
            wLimits: 
            With(
               {
                  wLimit: 
                  Sort(
                     IdeasFE,
                     ShadowID,
                     SortOrder.Descending
                    )
               },
               RoundDown(
                  First(wLimit).ShadowID / 2000,
                  0
               ) + 1
            )
         },
         AddColumns(
            RenameColumns(
               Sequence(
                  wLimits,
                  0,
                  2000
               ),
               "Value",
               "LowID"
            ),
            "HighID",
            LowID + 2000
         )
      )
   },
   ForAll(
      wSets As MaxMin,
      Collect(
         colAllListHP,
         Filter(
         IdeasFE,
         ShadowID > MaxMin.LowID && ShadowID <= MaxMin.HighID && 
         Fecha >= FI.SelectedDate && Fecha <= FF.SelectedDate
         )
      )
   )
)
