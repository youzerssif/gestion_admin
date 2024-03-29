# gestion_admin
### lien utile 
https://python.doctor/page-django-interface-admin-administration-settings-django-contrib-auth
https://blog.mounirmesselmeni.de/2016/03/21/how-to-order-a-calculated-count-field-in-djangos-admin/

```python
  from django.contrib import admin
  #-------Importation----------#
  from . import models
  from django.utils.safestring import mark_safe

  #-----------Register inline--------------#
  #-----------Permet lister d'autre(s) ModelAdmin associé à celui-ci--------------#

  class SousCategorieInline(admin.TabularInline):
      model =  models.SousCategorie
      #-------------le nombre d element a ajouter----------------#
      extra = 0

  class produitInline(admin.TabularInline):
      model = models.Produit
      extra = 0





  #------------register simple------------#

  @admin.register(models.Categorie)
  class CategorieAdmin(admin.ModelAdmin):
  #-----------Permet lister SousCategorieInline ModelAdmin associé à CategorieAdmin--------------#

      inlines = [SousCategorieInline]
  #-----------Permet d afficher les champs dans l admin--------------#

      list_display = (
          'view_image',
          'nom',
          'status',
          'date_add',
          'date_upd',
      )
  #-----------Permet de filtrer les champs dans l admin--------------#
      list_filter = (
          'status',
          'date_add',
          'date_upd',
      )

      #-------- fonction pour afficher une image dans l admin-----------------#
      def view_image(self,obj):

          return mark_safe('<img src="{img_url}" width="100px", heigth="100px"/>'.format(img_url=obj.image.url))

          #-------- fonction pour afficher une image dans l admin partie des inline-----------------#

      def detail_image(self,obj):

          return mark_safe('<img src="{img_url}" width="100px", heigth="100px"/>'.format(img_url=obj.image.url))

      #-------------------- Permet de Créer un search box dans la page de listing------------------#
      search_fields = ('nom',)
      date_hierarchy = 'date_add'
      #---------pour personnaliser les actions de l admin-------------#
      actions = (
          'active',
          'desactive',
      )
      #--------Pour rendre un element cliquable. tous les elements sauf standards ----------------#
      list_display_links = [
          'view_image',
          'nom',
          ]

      #--------------Pour afficher le nombre d element par Page---------------------#
      list_per_page = 3

      #------------- Ordonner par ordre alphabetique ---------------#
      ordering = [
          'nom',
      ]
      #----------------------- il permet d afficher l image dans l admin cote tabulariline-----------#
      readonly_fields = ['detail_image']

      #----------------------- Definition des fonctions pour gerer les actions-----------#
      def active(self,request,queryset):
          queryset.update(status=True)
          self.message_user(request,"la selection a ete activer avec succes")
      active.short_description = "Activer les Categorie selectionner"

      def desactive(self,request,queryset):
          queryset.update(status=False)
          self.message_user(request,"la selection a ete desactiver avec succes")
      desactive.short_description = "desactiver les Categorie selectionner"


  @admin.register(models.SousCategorie)
  class SousCategorieAdmin(admin.ModelAdmin):
      inlines = [ProduitIline]
      list_display = (
          'categorie',
          'nom',
          'status',
          'date_add',
          'date_upd',
          'view_image',
      )
      list_filter = (
          'status',
          'date_add',
          'date_upd',
          'categorie',
      )
      readonly_field = ['detail_image']

      search_fields = ('nom',)
      date_hierarchy = 'date_add'
      #---------pour personnaliser les actions de l admin-------------#
      actions = (
          'active',
          'desactive',
      )
      #--------Pour rendre un element cliquable. tous les elements sauf standards ----------------#
      list_display_links = [
          'view_image',
          'nom',
          ]

      #--------------Pour afficher le nombre d element par Page---------------------#
      list_per_page = 3

      #------------- Ordonner par ordre alphabetique ---------------#
      ordering = [
          'nom',
      ]
      #-------- fonction pour afficher une image dans l admin-----------------#
      def view_image(self,obj):

          return mark_safe('<img src="{img_url}" width="100px", heigth="100px"/>'.format(img_url=obj.image.url))

      def detail_image(self,obj):

          return mark_safe('<img src="{img_url}" width="100px", heigth="100px"/>'.format(img_url=obj.image.url))


      def active(self,request,queryset):
          queryset.update(status=True)
          self.message_user(request,"la selection a ete activer avec succes")
      active.short_description = "Activer les SousCategorie selectionner"

      def desactive(self,request,queryset):
          queryset.update(status=False)
          self.message_user(request,"la selection a ete desactiver avec succes")
      desactive.short_description = "desactiver les SousCategorie selectionner"

  @admin.register(models.Produit)
  class ProduitAdmin(admin.ModelAdmin):
      list_display = (
          'sous_cat',
          'titre',
          'status',
          'date_add',
          'date_updt',
          'image',
          'view_image',

      )
      list_filter = (
      'status',
      'date_add',
      'date_updt',
      'sous_cat',
      'tag',
      )
      def view_image(self,obj):

          return mark_safe('<img src="{img_url}" width="100px", heigth="100px"/>'.format(img_url=obj.image.url))

      def detail_image(self,obj):

          return mark_safe('<img src="{img_url}" width="100px", heigth="100px"/>'.format(img_url=obj.image.url))

      readonly_field = ['detail_image']

      search_fields = ('titre',)
      date_hierarchy = 'date_add'
      #---------pour personnaliser les actions de l admin-------------#
      actions = (
          'active',
          'desactive',
      )
      #--------Pour rendre un element cliquable. tous les elements sauf standards ----------------#
      list_display_links = [
          'view_image',
          'titre',
          ]

      #--------------Pour afficher le nombre d element par Page---------------------#
      list_per_page = 3

      #------------- Ordonner par ordre alphabetique ---------------#
      ordering = [
          'titre',
      ]
      #--------------- Permet de creer de regrouper des champ en compatiment dans l admin -------------#
      fieldsets = [
          ('titre et visibilite',{'fields':['titre','status']}),
          ('description et image',{'fields':['description','image']}),
          ('tag et sous categorie',{'fields':['tag','sous_cat']})
      ]
      #--------------- Permet de filtrer horizontal pour le many-to-many--------------------#
      filter_horizontal = ('tag',)

      #-------- fonction pour afficher une image dans l admin-----------------#



      def active(self,request,queryset):
          queryset.update(status=True)
          self.message_user(request,"la selection a ete activer avec succes")
      active.short_description = "Activer les SousCategorie selectionner"

      def desactive(self,request,queryset):
          queryset.update(status=False)
          self.message_user(request,"la selection a ete desactiver avec succes")
      desactive.short_description = "desactiver les SousCategorie selectionner"

  @admin.register(models.Tag)
  class TagAdmin(admin.ModelAdmin):
      list_display = (
          'nom',
          'status',
          'date_add',
          'date_updt',

      )
      list_filter = (
      'status',
      'date_add',
      'date_updt',
      'nom',
      )


      readonly_field = ['detail_image']

      search_fields = ('nom',)
      date_hierarchy = 'date_add'
      #---------pour personnaliser les actions de l admin-------------#
      actions = (
          'active',
          'desactive',
      )
      #--------Pour rendre un element cliquable. tous les elements sauf standards ----------------#
      list_display_links = [
          'nom',
          ]

      #--------------Pour afficher le nombre d element par Page---------------------#
      list_per_page = 3

      #------------- Ordonner par ordre alphabetique ---------------#
      ordering = [
          'nom',
      ]
      #-------- fonction pour afficher une image dans l admin-----------------#



      def active(self,request,queryset):
          queryset.update(status=True)
          self.message_user(request,"la selection a ete activer avec succes")
      active.short_description = "Activer les SousCategorie selectionner"

      def desactive(self,request,queryset):
          queryset.update(status=False)
          self.message_user(request,"la selection a ete desactiver avec succes")
      desactive.short_description = "desactiver les SousCategorie selectionner"

  ```
