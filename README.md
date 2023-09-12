# Medico 
# A new aspect towards medicine inventory management system

   

<div> <img src="https://github.com/tech-jamara/PharmacyManagementSystem/blob/main/screenshot/Admin%20Login.png" width="400" height="200" />
<img src="https://github.com/tech-jamara/PharmacyManagementSystem/blob/main/screenshot/Pharmacist.png" width="400" height="200" />
</div>
<div> <img src="https://github.com/tech-jamara/PharmacyManagementSystem/blob/main/screenshot/Doctor%20Login.png" width="400" height="200" />
<img src="https://github.com/tech-jamara/PharmacyManagementSystem/blob/main/screenshot/Receptionist%20Login.png" width="400" height="200" />
    <img src="https://github.com/tech-jamara/PharmacyManagementSystem/blob/main/screenshot/Patient%20login.png" width="400" height="200" />
</div>

--------------------------------------------------------------------------------------
# Create a new medicine
```@app.route(&#39;/medicine&#39;, methods=[&#39;POST&#39;])
def add_medicine():
name = request.json[&#39;name&#39;]
manufacturer = request.json[&#39;manufacturer&#39;]
quantity = request.json[&#39;quantity&#39;]

medicine = {
&#39;name&#39;: name,
&#39;manufacturer&#39;: manufacturer,
&#39;quantity&#39;: quantity
}

mongo.db.medicines.insert_one(medicine)

return jsonify({&#39;message&#39;: &#39;Medicine added successfully&#39;}), 201
```

# Get all medicines
```@app.route(&#39;/medicines&#39;, methods=[&#39;GET&#39;])
def get_medicines():
medicines = mongo.db.medicines.find()
result = []
for medicine in medicines:
result.append({
&#39;id&#39;: str(medicine[&#39;_id&#39;]),
&#39;name&#39;: medicine[&#39;name&#39;],
&#39;manufacturer&#39;: medicine[&#39;manufacturer&#39;],
&#39;quantity&#39;: medicine[&#39;quantity&#39;]
})
return jsonify(result)
```

# Get a single medicine by ID
```@app.route(&#39;/medicine/&lt;id&gt;&#39;, methods=[&#39;GET&#39;])
def get_medicine(id):
medicine = mongo.db.medicines.find_one({&#39;_id&#39;: ObjectId(id)})
if medicine:
return jsonify({
&#39;id&#39;: str(medicine[&#39;_id&#39;]),
&#39;name&#39;: medicine[&#39;name&#39;],
&#39;manufacturer&#39;: medicine[&#39;manufacturer&#39;],
&#39;quantity&#39;: medicine[&#39;quantity&#39;]
})
return jsonify({&#39;message&#39;: &#39;Medicine not found&#39;}), 404
```

# Update a medicine by ID
```@app.route(&#39;/medicine/&lt;id&gt;&#39;, methods=[&#39;PUT&#39;])
def update_medicine(id):
name = request.json[&#39;name&#39;]
manufacturer = request.json[&#39;manufacturer&#39;]
quantity = request.json[&#39;quantity&#39;]

mongo.db.medicines.update_one({&#39;_id&#39;: ObjectId(id)}, {&#39;$set&#39;: {
&#39;name&#39;: name,
&#39;manufacturer&#39;: manufacturer,
&#39;quantity&#39;: quantity
}})

return jsonify({&#39;message&#39;: &#39;Medicine updated successfully&#39;})
```

# Delete a medicine by ID
```
@app.route(&#39;/medicine/&lt;id&gt;&#39;, methods=[&#39;DELETE&#39;])
def delete_medicine(id):
mongo.db.medicines.delete_one({&#39;_id&#39;: ObjectId(id)})
return jsonify({&#39;message&#39;: &#39;Medicine deleted successfully&#39;})

#Search Medicines by Name:
@app.route(&#39;/medicines/search/&lt;keyword&gt;&#39;, methods=[&#39;GET&#39;])
def search_medicines(keyword):
medicines = mongo.db.medicines.find({&quot;name&quot;: {&quot;$regex&quot;: keyword, &quot;$options&quot;: &quot;i&quot;}})
result = []
for medicine in medicines:
result.append({
&#39;id&#39;: str(medicine[&#39;_id&#39;]),
&#39;name&#39;: medicine[&#39;name&#39;],
&#39;manufacturer&#39;: medicine[&#39;manufacturer&#39;],
&#39;quantity&#39;: medicine[&#39;quantity&#39;]
})
return jsonify(result)
```

#Sort Medicines by Quantity:
```
@app.route(&#39;/medicines/sort/quantity&#39;, methods=[&#39;GET&#39;])
def sort_medicines_by_quantity():
medicines = mongo.db.medicines.find().sort(&#39;quantity&#39;, 1) # 1 for ascending, -1 for
descending
result = []
for medicine in medicines:
result.append({
&#39;id&#39;: str(medicine[&#39;_id&#39;]),
&#39;name&#39;: medicine[&#39;name&#39;],
&#39;manufacturer&#39;: medicine[&#39;manufacturer&#39;],
&#39;quantity&#39;: medicine[&#39;quantity&#39;]
})
return jsonify(result)
```

#Get Medicine Statistics (Total Quantity):
```
@app.route(&#39;/medicine/statistics&#39;, methods=[&#39;GET&#39;])
def get_medicine_statistics():
total_quantity = mongo.db.medicines.aggregate([
{&quot;$group&quot;: {&quot;_id&quot;: None, &quot;total_quantity&quot;: {&quot;$sum&quot;: &quot;$quantity&quot;}}}
])
total_quantity = list(total_quantity)[0][&#39;total_quantity&#39;] if total_quantity else 0
return jsonify({&#39;total_quantity&#39;: total_quantity})
```










