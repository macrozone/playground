	
	
	
	Router.map -> 
		@route 'boxes',
		data: ->
			boxes: Boxes.find {parent: null}, sort: sorting: 1


	Template.box.sizes = ->
		[1..12]

	Template.box.selectedWidth = (width) ->

		if (width == @valueOf())

			return "selected "
		else 
			return ""

	Template.box.boxes = ->
		Boxes.find {parent: @_id}, sort: sorting: 1


	Template.editableFieldTemplate.content = ->
		
		if @content? and @content.length > 0
			@content
		else
			"(Click to edit)" 
	Template.box.editableField = (property) ->
		@property = property
		Template.editableFieldTemplate @

	Template.editableFieldTemplate.events
		"click .editContent": (event, template)->
			$box = $(event.target).parentsUntil(".box").parent()
			$box.addClass "editMode"
		"blur .editContent": (event, template)->
			$box = $(event.target).parentsUntil(".box").parent()
			$box.removeClass "editMode"

		"change .editContent": (event, template) ->
			
			$set = {}
			$set[@property] = $(event.target).val()
			Boxes.update {_id: template.data._id}, $set: $set

		

	Template.boxes.events
		"click .btn-add-box": (event, template)->
			
			Boxes.insert 
				width: 6
				sorting: 0
			false
		
	Template.box.events

		"drop .box": (event, template) ->
			
			fromID = event.dataTransfer.getData 'ID'
			
			
			

			Boxes.update {_id: fromID}, $set: parent: @parent, sorting: @sorting+1000
			return false
		"dragover .box": (event, template) ->
			event.preventDefault()
			#console.log event, template
		"dragstart": (event, template) ->
			 event.dataTransfer.setData 'ID', @_id
		"click .btn-add-child": (event, template)->
			# move to content to the new child
			# a box with subboxes cant have content
			Boxes.update {_id: @_id}, $unset: content: 1
		
			Boxes.insert 
				width: 6
				parent: @_id
				content: @content
				sorting: 0
			false
		"click .btn-add-sibling": (event, template)->
			sorting = @sorting ? 0
			sorting +=1000
			console.log sorting
			Boxes.insert 
				width: 6
				parent: @parent
				sorting: sorting
			false
		"click .btn-remove-box": (event, template) ->
			
			# give the children a new parent
			Boxes.find(parent: @_id).map (box) => 
				if @parent?
					Boxes.update {_id: box._id}, $set: parent: @parent
				else
					Boxes.update {_id: box._id}, $unset: parent: 1
			if @parent?
				Boxes.update {_id: @parent}, $set: content: @content

			Boxes.remove _id: @_id



	Template.box.events

		"change .width": (event, template) ->
			Boxes.update {_id: @_id}, $set: width: parseInt $(event.target).val(),10
		"change .height": (event, template) ->
			Boxes.update {_id: @_id}, $set: height: $(event.target).val()
		"click": (event)->
			$(".box").not(event.target).removeClass "active"
			$(event.target).toggleClass "active"

		
			
