App.jsx

import { useEffect, useState} from "react";
import axios from "axios";
import './App.css'

function App() {
  const [users, setUsers] = useState([]);
  const [filterusers, setFilterusers] = useState([]);
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [userData, setUserData] = useState({task:"",priority:"",assigned:"",completed:""});


  const getAllUsers= async () =>{
    await axios.get("http://localhost:8000/users").then
    ((res)=>{
      console.log(res.data);
      setUsers(res.data);
      setFilterusers(res.data);
    });
  };
  useEffect(() => {
    getAllUsers();
  }, []);

  const handleSearchChange=(e)=>{
    const searchText=e.target.value.toLowerCase();
    const filteredUsers=users.filter((user)=>user.task.toLowerCase().includes(searchText)||user.completed.toLowerCase().includes(searchText));
    setFilterusers(filteredUsers);
  };
  const handleDelete=async (id)=>{
    await axios.delete(`http://localhost:8000/users/${id}`).then((res)=>{
      setUsers(res.data);
      setFilterusers(res.data);
    });
  };
const handleAddRecord = () =>{
setUserData({task:"",priority:"",assigned:"",completed:""});
setIsModalOpen(true);
};

  return (
    <>
      <div className="container">
        <h3>CRUD Application</h3>
        <div className="input-search">
          <input type="search" placeholder="Search" onchange={handleSearchChange}/>
          <button onClick={handleAddRecord}>Add Record</button>
        </div>
        <table className="table">
          <thead>
            <tr>
              <th>Id</th>
              <th>Task</th>
              <th>Priority</th>
              <th>Assigned</th>
              <th>Completed</th>
              <th>Edit</th>
              <th>Delete</th>
            </tr>
          </thead>
          <tbody>
            {filterusers && 
            filterusers.map((user,index)=>{
              return (
              <tr key={user.id}>
              <td>{index + 1}</td>
              <td>{user.task}</td>
              <td>{user.priority}</td>
              <td>{user.assigned}</td>
              <td>{user.completed}</td>
              <td>
                <button className="btn green">Edit</button>
              </td>
              <td>
                <button onClick={()=>handleDelete(user.id)}
                className="btn red">Delete</button>
              </td>
            </tr>
              );
            })}
           
          </tbody>
        </table>
        {isModalOpen && ( 
          <div className="modal">
            <div className="modal-content">
              <h2>Record</h2>
            </div>
            </div>
        )}
      </div>
      
    </>
  )
}

export default App
