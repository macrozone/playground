


	Router.map -> @route 'gravitation'

	Template.gravitation.rendered = ->

		console.log @		

		TIME_STEP = 0.1
		STEPS_PER_FRAME = 100



		sample1 = [
			(
				c: "green"
				m: 5
				x: new Vector [200,200] 
				v: new Vector [0.1,-0.1]
			)
			(
				c: "red"
				m: 5
				x: new Vector [100,100]
				v: new Vector [0,0.1]
			)
			]



		sample2 = for i in [0..100]
			x = _.random 200,400
			y = _.random 200,400
			vx = Math.random()-0.5
			vy = Math.random()-0.5
			object = 
				m: 1
				x: new Vector [x,y]
				v: new Vector [vx,vy]


		sample3 = [
			(
				c: "magenta"
				m: 0.01
				x: new Vector [300,600] 
				v: new Vector [-1.5,0]
			)
			(
				c: "orange"
				m: 0.1
				x: new Vector [400,400] 
				v: new Vector [0,1]
			)
			(
				c: "green"
				m: 0.1
				x: new Vector [400,100] 
				v: new Vector [0,0.7]
			)
			(
				c: "blue"
				m: 0.1
				x: new Vector [100,200]
				v: new Vector [0,4]
			)
			(
				c: "red"
				m: 1000
				x: new Vector [200,200]
				v: new Vector [0,0]
			)
			]



		canvas = @find "canvas"
		if canvas?
			context = canvas.getContext "2d"
		context.fillStyle = "red"


		offsetX = 200
		offsetY = 200
		draw = (objects) ->
			for obj in objects
				if obj.c? then context.fillStyle = obj.c
				context.fillRect(offsetX + obj.x.data[0],offsetY + obj.x.data[1],1,1)


		run = (objects) ->
			step = ->
				for i in [0...STEPS_PER_FRAME]
					GravitationEngine.step objects, TIME_STEP
					draw objects
				_.defer step
			step()

		


start it
		
		run sample3



	
